<!--
author:   David Croft

email:    david.croft@warwick.ac.uk

attribute: Based on the work of the [DART Team](dart@chop.edu) available [here](https://raw.githubusercontent.com/arcus/education_modules/main/_module_templates/macros_sql.md)

version:  0.0.1

language: en

narrator: US English Female

comment:  

classroom: false
icon: https://dscroft.github.io/liascript_materials/assets/logo.svg


script: https://cdn.jsdelivr.net/npm/alasql@0.6.5/dist/alasql.min.js
attribute: [AlaSQL](https://alasql.org)
           by [Andrey Gershun](agershun@gmail.com)
           & [Mathias Rangel Wulff](m@rawu.dk)
           is licensed under [MIT](https://opensource.org/licenses/MIT)

script: https://cdnjs.cloudflare.com/ajax/libs/PapaParse/4.6.1/papaparse.min.js
attribute: [PapaParse](https://www.papaparse.com)
           by [Matthew Holt](https://twitter.com/mholt6)
           is licensed under [MIT](https://opensource.org/licenses/MIT)

script: https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js
attribute: [jQuery](https://jquery.com/)
           is licensed under [OpenJS Foundation](https://openjsf.org/)

@style
  .browser-window {
    max-width: 800px;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: white;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    overflow: hidden;
  }

  .browser-header {
    display: flex;
    align-items: center;
    background-color: #e5e7eb;
    padding: 8px 12px;
  }

  .browser-buttons {
    display: flex;
    gap: 6px;
    margin-right: 12px;
  }

  .browser-button {
    width: 12px;
    height: 12px;
    border-radius: 50%;
  }

  .red { background-color: #ff5f56; }
  .yellow { background-color: #ffbd2e; }
  .green { background-color: #27c93f; }

  .header-title {
    font-weight: bold;
    font-size: 16px;
  }

  .content {
    display: flex;
    padding: 20px;
  }

  .form-section {
    flex: 1;
  }

  .form-section label {
    font-weight: bold;
  }

  .logo-section {
    flex: 0 0 40%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding-left: 20px;
  }

  .logo-img {
      max-width: 100%;
      height: auto;
      border-radius: 8px;
      background-color: black;
    }
@end

@LoginExample
SQL query being run:

<strong id="loginQuery"></strong><strong id="loginComment" style="color: lightgrey"></strong><br>

----------------------------------

<div class="browser-window">
  <div class="browser-header">
    <div class="browser-buttons">
      <div class="browser-button red"></div>
      <div class="browser-button yellow"></div>
      <div class="browser-button green"></div>
    </div>
    <div class="header-title">Sign in page</div>
  </div>
  <div class="content">
    <div class="form-section">
      <form autocomplete="off">
        <label for="username">Username:</label><br>
        <input class="lia-quiz__input" type="text" id="username"><br>
        <label for="password">Password:</label><br>
        <input class="lia-quiz__input" type="password" id="password" value=""><br><br>
        <input class="lia-btn--outline" type="button" id="login" value="Login">
        <span style="display:inline-block; width: 20px;"></span>
        <strong id="status"></strong>
      </form>
    </div>
    <div class="logo-section">
      <img src="media/cyberdyne.webp" alt="Cyberdyne Logo" class="logo-img">
    </div>
  </div>
</div>

<script>
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Generate a login form with an SQLi vulnerability and validate it against the ALASQL database.
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    function update_query()
    {
        let username = document.getElementById("username").value;
        let password = document.getElementById("password").value;
        
        let query = "SELECT COUNT(*) FROM users WHERE username = '" + username + "' AND password = '" + password + "';";

        // shade the comments
        const reg = /(.*)(--.*)/;
        const match = query.match( reg );
        if( match )
        {
          document.getElementById("loginQuery").textContent = match[1];
          document.getElementById("loginComment").textContent = match[2];
        }
        else
        {
          document.getElementById("loginQuery").textContent = query;
          document.getElementById("loginComment").textContent = "";
        }
    }

    function login()
    {
        try 
        {
            let status = document.getElementById("status");

            // default 
            status.innerHTML = "Error";
            status.style.color = "orange";

            let query = document.getElementById("loginQuery").innerHTML;
            let result = alasql(query);

            if( result[0]["COUNT(*)"] > 0 )
            {
                status.innerHTML = "Logged in successfully!";
                status.style.color = "green";
            }
            else
            {
                status.innerHTML = "Login failed!";
                status.style.color = "red";
            }
        } 
        catch(e) 
        {
            let error = new LiaError(e.message, 1);
            try {
            let log = e.message.match(/.*line (\d):.*\n.*\n.*\n(.*)/);
            error.add_detail(0, e.name+": "+log[2], "error", log[1] -1 , 0);
            } 
            catch(e){}

            throw error;
        }
      }

    document.getElementById("username").addEventListener("input", update_query);
    document.getElementById("password").addEventListener("input", update_query);
    document.getElementById("login").addEventListener("click", login);

    update_query();
</script>

@end

@SearchExample
SQL query being run:

<strong id="searchQuery"></strong><strong id="searchComment" style="color: lightgrey"></strong><br>

--------------------

<div class="browser-window">
  <div class="browser-header">
    <div class="browser-buttons">
      <div class="browser-button red"></div>
      <div class="browser-button yellow"></div>
      <div class="browser-button green"></div>
    </div>
    <div class="header-title">Search page</div>
  </div>
  <div class="content" style="align-items: flex-start;">
    <div class="form-section" style="flex: 1;">
      <form autocomplete="off">
        <input class="lia-quiz__input" type="text" id="registration" placeholder="Enter reg"><br><br>
        <input class="lia-btn--outline" type="button" id="search" value="Search">
        <span style="display:inline-block; width: 20px;"></span>
        <strong id="status"></strong>
      </form>
      <div id="searchResults" style="padding: 16px; overflow-x: auto;"></div>
    </div>
    <div class="logo-section" style="align-self: flex-start;">
      <img src="media/cyberdyne.webp" alt="Cyberdyne Logo" class="logo-img">
    </div>
  </div>
</div>



<script>
    function update_query()
    {
        let registration = document.getElementById("registration").value;
        
        let query = "SELECT reg, color, make, model FROM vehicles WHERE reg = '" + registration + "';";

        const reg = /(.*)(--.*)/;
        const match = query.match( reg );
        if( match )
        {
          document.getElementById("searchQuery").textContent = match[1];
          document.getElementById("searchComment").textContent = match[2];
        }
        else
        {
          document.getElementById("searchQuery").textContent = query;
          document.getElementById("searchComment").textContent = "";
        }
    }

    function search()
    {
        try 
        {
            let query = document.getElementById("searchQuery").innerHTML;
            let result = alasql(query);

            console.log( result )

            let status = document.getElementById("searchResults");
            if( result.length > 0 )
            {
                status.innerHTML = "<h3>Results:</h3>";
                for( let i=0; i<result.length; i+=1 )
                {
                  status.innerHTML += "<span style='display:inline-block; min-width:100px; font-size:1.2em; font-weight:bold;'>" + result[i].reg + "</span> " + result[i].color + " " + result[i].make + " " + result[i].model + "<br>";
                }
                status.style.color = "";
            }
            else
            {
                status.innerHTML = "No vehicles found!";
                status.style.color = "red";
            }
        } 
        catch(e) 
        {
          let error = new LiaError(e.message, 1);
          try {
            let log = e.message.match(/.*line (\d):.*\n.*\n.*\n(.*)/);
            error.add_detail(0, e.name+": "+log[2], "error", log[1] -1 , 0);
          } 
          catch(e){}

          throw error;
        }
      }

    document.getElementById("registration").addEventListener("input", update_query);
    document.getElementById("search").addEventListener("click", search);

    update_query();
</script>

@end

@onload
    console.log("Building users table");
    alasql("DROP TABLE IF EXISTS users;");
    alasql("CREATE TABLE users (id number PRIMARY KEY AUTOINCREMENT, username text, password text, firstname text, lastname text, address text, city text, county text, postal text, phone text, email text);");
    alasql("INSERT INTO users VALUES ('1000','Trout393','steal3','Aleshia','Tomkiewicz','14 Taylor St','St. Stephens Ward','Kent','CT2 7PP','01835-703597','atomkiewicz@hotmail.com');");
    alasql("INSERT INTO users VALUES ('1017','Pigeon729','nose8','Evan','Zigomalas','5 Binney St','Abbey Ward','Buckinghamshire','HP11 2AX','01937-864715','evan.zigomalas@gmail.com');");
    alasql("INSERT INTO users VALUES ('1034','Aardwolf536','party9','France','Andrade','8 Moor Place','East Southbourne and Tuckton W','Bournemouth','BH6 3BE','01347-368222','france.andrade@hotmail.com');");
    alasql("INSERT INTO users VALUES ('1051','Stoat334','brown0','Ulysses','Mcwalters','505 Exeter Rd','Hawerby cum Beesby','Lincolnshire','DN36 5RP','01912-771311','ulysses@hotmail.com');");
    alasql("INSERT INTO users VALUES ('1068','Toucan875','ought3','Tyisha','Veness','5396 Forth Street','Greets Green and Lyng Ward','West Midlands','B70 9DT','01547-429341','tyisha.veness@hotmail.com');");
    alasql("INSERT INTO users VALUES ('1085','Ferret818','share0','Eric','Rampy','9472 Lind St','Desborough','Northamptonshire','NN14 2GH','01969-886290','erampy@rampy.co.uk');");
    alasql("INSERT INTO users VALUES ('1102','Walrus532','stand5','Marg','Grasmick','7457 Cowl St','Bargate Ward','Southampton','SO14 3TY','01865-582516','marg@hotmail.com');");
    alasql("INSERT INTO users VALUES ('1119','Finch151','human7','Laquita','Hisaw','20 Gloucester Pl','Chirton Ward','Tyne & Wear','NE29 7AD','01746-394243','laquita@yahoo.com');");
    alasql("INSERT INTO users VALUES ('1136','Roadrunner940','solve1','Lura','Manzella','929 Augustine St','Staple Hill Ward','South Gloucestershire','BS16 4LL','01907-538509','lura@hotmail.com');");
    alasql("INSERT INTO users VALUES ('1153','Falcon405','chair8','Yuette','Klapec','45 Bradfield St','Parwich','Derbyshire','DE6 1QN','01903-649460','yuette.klapec@klapec.co.uk');");
    alasql("INSERT INTO users VALUES ('1170','Snail980','lower9','Fernanda','Writer','620 Northampton St','Wilmington','Kent','DA2 7PP','01630-202053','fernanda@writer.co.uk');");
    alasql("INSERT INTO users VALUES ('1187','Shark125','power8','Charlesetta','Erm','5 Hygeia St','Loundsley Green Ward','Derbyshire','S40 4LY','01276-816806','charlesetta_erm@gmail.com');");
    alasql("INSERT INTO users VALUES ('1204','Moose147','hunter2','Corrinne','Jaret','2150 Morley St','Dee Ward','Dumfries and Galloway','DG8 7DE','01625-932209','corrinne_jaret@gmail.com');");
    alasql("INSERT INTO users VALUES ('1221','Hoverfly591','might3','Niesha','Bruch','24 Bolton St','Broxburn, Uphall and Winchburg','West Lothian','EH52 5TL','01874-856950','niesha.bruch@yahoo.com');");
    alasql("INSERT INTO users VALUES ('1238','Magpie205','cross1','Rueben','Gastellum','4 Forrest St','Weston-Super-Mare','North Somerset','BS23 3HG','01976-755279','rueben_gastellum@gastellum.co.uk');");
    alasql("INSERT INTO users VALUES ('1272','Horse885','mess2','Edgar','Kanne','99 Guthrie St','New Milton','Hampshire','BH25 5DF','01326-532337','edgar.kanne@yahoo.com');");
    alasql("INSERT INTO users VALUES ('1289','Gibbon766','kind0','Dewitt','Julio','7 Richmond St','Parkham','Devon','EX39 5DJ','01253-528327','dewitt.julio@hotmail.com');");
    alasql("INSERT INTO users VALUES ('1306','Octopus498','joint5','Charisse','Spinello','9165 Primrose St','Darnall Ward','Yorkshire, South','S4 7WN','01719-831436','charisse_spinello@spinello.co.uk');");
    alasql("INSERT INTO users VALUES ('1340','Rhinoceros275','hair3','Peter','Gutierres','4410 Tarlton St','Prestatyn Community','Denbighshire','LL19 9EG','01842-767201','peter_gutierres@yahoo.com');");
    alasql("INSERT INTO users VALUES ('1357','Tyrannosaurus899','read6','Octavio','Salvadore','6949 Bourne St','Lye and Stourbridge North Ward','West Midlands','DY5 2QP','01552-709248','octavio.salvadore@yahoo.com');");
    console.log("Table users created");

    console.log("Building vehicles table");
    alasql("DROP TABLE IF EXISTS vehicles;");
    alasql("CREATE TABLE vehicles (vin text PRIMARY KEY, make text, model text, year number, reg text, color text);");
    alasql("INSERT INTO vehicles VALUES ('3GCPCSE05BG193249', 'Chevrolet', 'Silverado', 2011, 'HW21 FKL', 'Black');");
    alasql("INSERT INTO vehicles VALUES ('1HGCM82633A123456', 'Honda', 'Accord', 2003, 'JK12 WZX', 'Blue');");
    alasql("INSERT INTO vehicles VALUES ('4T1BE32K44U876543', 'Toyota', 'Camry', 2004, 'XY34 FSD', 'Silver');");
    alasql("INSERT INTO vehicles VALUES ('2C3KA43R88H125478', 'Chrysler', '300', 2008, 'GH56 BNB', 'Gray');");
    alasql("INSERT INTO vehicles VALUES ('1FTFW1ET0EKE39357', 'Ford', 'F-150', 2014, 'LM98 QWE', 'White');");
    alasql("INSERT INTO vehicles VALUES ('5UXWX9C5XH0D12345', 'BMW', 'X3', 2017, 'HZ76 YUT', 'Blue');");
    alasql("INSERT INTO vehicles VALUES ('3FA6P0HR1GR187654', 'Ford', 'Fusion', 2016, 'RJ20 OPL', 'Red');");
    alasql("INSERT INTO vehicles VALUES ('5YJ3E1EA7KF314159', 'Tesla', 'Model 3', 2019, 'PR23 GHJ', 'White');");
    alasql("INSERT INTO vehicles VALUES ('1G1BE5SM2G7101234', 'Batmobile', 'Classic', 1966, 'BAT MAN', 'Black');");
    alasql("INSERT INTO vehicles VALUES ('WAUFFAFL7CN111111', 'Audi', 'A4', 2012, 'WD76 QPA', 'Black');");
    alasql("INSERT INTO vehicles VALUES ('2HGFB2F50FH123789', 'Honda', 'Civic', 2015, 'ZX34 NML', 'Gray');");
    alasql("INSERT INTO vehicles VALUES ('JHMGE8H39CS004321', 'Honda', 'Fit', 2012, 'LK98 VFR', 'Green');");
    alasql("INSERT INTO vehicles VALUES ('1FTEX1EP3FKD56789', 'Ford', 'F-150', 2015, 'QW89 POI', 'Red');");
    alasql("INSERT INTO vehicles VALUES ('WBA3A5C55FF123456', 'BMW', '3 Series', 2015, 'TY67 UIY', 'White');");
    alasql("INSERT INTO vehicles VALUES ('JN1CV6EK9EM123456', 'Infiniti', 'G37', 2014, 'NB76 MNO', 'Black');");
    alasql("INSERT INTO vehicles VALUES ('3N1AB7AP4HY123789', 'Nissan', 'Sentra', 2017, 'HG12 VBG', 'Blue');");
    alasql("INSERT INTO vehicles VALUES ('1FAFP40454F123456', 'Ford', 'Mustang', 2008, 'OL56 NRT', 'Yellow');");
    alasql("INSERT INTO vehicles VALUES ('1G11C5SA2EF204321', 'Chevrolet', 'Malibu', 2014, 'YX34 HJK', 'Gray');");
    alasql("INSERT INTO vehicles VALUES ('19UUA8F55CA001234', 'Acura', 'TL', 2012, 'UJ98 XCN', 'Black');");
    alasql("INSERT INTO vehicles VALUES ('JTHBK1EG3C2098765', 'Lexus', 'ES 350', 2012, 'DM56 KOP', 'Silver');");
    console.log("Table vehicles created");

    console.log("Building owners table");
    alasql("DROP TABLE IF EXISTS owners;");
    alasql("CREATE TABLE owners (userid number, vin text);");
    alasql("INSERT INTO owners VALUES ('1', '3GCPCSE05BG193249');");
    alasql("INSERT INTO owners VALUES ('2', '1HGCM82633A123456');");
    alasql("INSERT INTO owners VALUES ('3', '4T1BE32K44U876543');");
    alasql("INSERT INTO owners VALUES ('4', '2C3KA43R88H125478');");
    alasql("INSERT INTO owners VALUES ('4', '1FTFW1ET0EKE39357');");
    alasql("INSERT INTO owners VALUES ('4', '5UXWX9C5XH0D12345');");
    alasql("INSERT INTO owners VALUES ('7', '3FA6P0HR1GR187654');");
    alasql("INSERT INTO owners VALUES ('8', '5YJ3E1EA7KF314159');");
    alasql("INSERT INTO owners VALUES ('9', '1G1BE5SM2G7101234');");
    alasql("INSERT INTO owners VALUES ('10', 'WAUFFAFL7CN111111');");
    alasql("INSERT INTO owners VALUES ('11', '2HGFB2F50FH123789');");
    alasql("INSERT INTO owners VALUES ('12', 'JHMGE8H39CS004321');");
    alasql("INSERT INTO owners VALUES ('13', '1FTEX1EP3FKD56789');");
    alasql("INSERT INTO owners VALUES ('14', 'WBA3A5C55FF123456');");
    alasql("INSERT INTO owners VALUES ('15', 'JN1CV6EK9EM123456');");
    alasql("INSERT INTO owners VALUES ('15', '3N1AB7AP4HY123789');");
    alasql("INSERT INTO owners VALUES ('17', '1FAFP40454F123456');");
    alasql("INSERT INTO owners VALUES ('18', '1G11C5SA2EF204321');");
    alasql("INSERT INTO owners VALUES ('19', '19UUA8F55CA001234');");
    alasql("INSERT INTO owners VALUES ('19', 'JTHBK1EG3C2098765');");
    console.log("Table owners created");
@end

@AlaSQL.eval
<script>
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// BUILD FUNCTIONS
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

function buildHtmlTable(myList) {
  // Builds the HTML Table out of myList, and writes output to the id attribute assigned via the "@0" argument to this marco.
  var columns = addAllColumnHeaders(myList);
  for (var i = 0 ; i < myList.length ; i++) {
    var row$ = $('<tr/>');
    for (var colIndex = 0 ; colIndex < columns.length ; colIndex++) {
      var cellValue = myList[i][columns[colIndex]];
      if (cellValue == null) { cellValue = ""; }
      row$.append($('<td/>').html(cellValue).css({
      "padding-left": "1em",
      "padding-right": "1em"
      }));
    }
    $(@0).append(row$);
  }
  try { // Error Handling for no null.
    var rowCount = document.getElementById(@0.substring(1)).rows.length - 1;
  } catch(err) {
    var cnt = 0
  }
  if (rowCount > 0) {
    var complete_message = "Query Execution Complete! (See Result Set Below)..."
  } else {
    var complete_message = "No Data to Return.."
  }
  return JSON.stringify(complete_message, null, 3);
}
function addAllColumnHeaders(myList) {
  // Creates and Returns Header Row From Array Data Provided as Input.
  var columnSet = [];
  var headerTr$ = $('<tr/>');
  for (var i = 0 ; i < myList.length ; i++) {
    var rowHash = myList[i];
    for (var key in rowHash) {
      if ($.inArray(key, columnSet) == -1){
        columnSet.push(key);
        headerTr$.append($('<th/>').html(key));
      }
    }
  }
  $(@0).append(headerTr$);
  return columnSet;
}
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
try {
    var myinput=`@input`
    console.clear();
    $(@0).html(""); // clear out existing data
    var myList=alasql(myinput);

    // no results returned
    if ( myList.length == 0 )
    {
      console.log("No Results to Display.");
    }
    // multiple queries returned, display final set
    else if ( Array.isArray( myList[0] ) )
    {
      buildHtmlTable(myList[myList.length-1]);
    }
    else
    { 
      buildHtmlTable(myList);
    }
} catch(e) {
  let error = new LiaError(e.message, 1);
  try {
    let log = e.message.match(/.*line (\d):.*\n.*\n.*\n(.*)/);
    error.add_detail(0, e.name+": "+log[2], "error", log[1] -1 , 0);
  } catch(e) {
  }
  throw error;
}
</script>
@end

-->

# Demo

@LoginExample