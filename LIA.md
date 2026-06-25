<!--
author:   David Croft
email:    david.croft@warwick.ac.uk
version:  0.2.0
language: en
narrator: UK English Female

classroom: enable
mode: Presentation
icon: https://dscroft.github.io/liascript_materials/assets/logo.svg

import: macros_interface.md
import: macros_dashboard.md

@style
.lia-effect__circle {
    display: none;
}

.flex-container {
    display: flex;
    flex-wrap: wrap; /* Allows the items to wrap as needed */
    align-items: stretch;
    gap: 20px; /* Adds both horizontal and vertical spacing between items */
}

.flex-child { 
    flex: 1;
    margin-right: 20px; /* Adds space between the columns */
}

@media (max-width: 600px) {
    .flex-child {
        flex: 100%; /* Makes the child divs take up the full width on slim devices */
        margin-right: 0; /* Removes the right margin */
    }
}
@end

@onload
// frame receiver
LIA.classroom.subscribe("can-frame", (message) => {
    window.can_message_handler(message["id"], message["data"]);
})

// frame sender
window.send_can_frame = function(frameid, data) {
    LIA.classroom.publish("can-frame", {
        id: frameid,
        data: data
    });
}
@end


@Classroom.defaultManager
------------------------

**CAN bus status: ** 
<script>
    function status()
    {
        if (LIA.classroom.connected) {
            send.lia("LIASCRIPT: **Emulated**<!-- style='color: green;' -->");
        } else {
            send.lia("LIASCRIPT: **Disconnected**<!-- style='color: red;' -->");
        }
    }

    setInterval(() => status, 1000);
    status();
</script> 

------------------------

@end
-->

# LiaScript for real and virtual hardware control

This presentation was created in [LiaScript](https://liascript.github.io/).

<section class="flex-container">
  <div class="flex-child">

    - If you want to follow along.
  </div>
  
  <div class="flex-child">
    [qr-code](https://liascript.github.io/course/?eyJiYWNrZW5kIjoiR1VOfGZ8aHR0cHM6Ly9ndW4ubzguaXMvZ3VuLCBodHRwczovL2d1bi5kZWZ1Y2MubWUvZ3VuLCBodHRwczovL3Nob2d1bi1yZWxheS5zY29icnVkb3QuZGV2L2d1biwgaHR0cHM6Ly9yZWxheS5wZWVyLm9vby9ndW4iLCJjb3Vyc2UiOiJodHRwczovL2RzY3JvZnQuZ2l0aHViLmlvL3BlZTI2L0xJQS5tZCIsInJvb20iOiJwZWUyNiJ9#1)
  </div>
</section>


# WMG

The Intelligent Vehicles (IV) teaching group within Warwick Manufacturing Group (WMG).

- Been delivering the Smart Connected Autonomous Vehicles (SCAV) maters programme.
- Begins offering CPD variant to a top level automotive company. 
  - 2024.
  - Additional commercially sensitive information.
- Delivered via WMG e-learning platform.
  - SCORM files on a secure Moodle.
  - Relied on a commercial suite of software tools. 

Delivery was a success.


## Maintenance

Regular updates.

- Specific changes requested by partner organisation.
  - Change management was a problem but to the platform's functionality.
  - Only one person able to work at a time.
- Specific incident.
  - Half the course lost day beforehand.
  - Backups meant no delivery impact.
  - Had to recreate.
  
Commercial suite might be acceptable for an individual.

- Does not work for a team.


As such members of the IV teaching group began to explore alternative software provisions. 
The loss of materials exemplified exactly the type of problem that robust version control is designed to eliminate, and so identifying an alternative platform which included or could support such functionality was a priority.
As version control systems function best with text based files, and as the group had prior experience with markdown based document generation, this led very quickly to the identification of LiaScript as a suitable alternative.

# LiaScript

LiaScript is an open-source framework for creating interactive learning materials. 

- Credit to André Dietrich.
- It allows educators to create engaging content using a simple markup language, making it easy to integrate multimedia elements and interactive features.

LiaScript is an open-source framework for creating interactive and multimedia-rich educational content using markdown. 
Although lacking some of the GUI development tools offered by the commercial suite, it offered dramatic improvements in collaborative development, version control and customisation. 
Indeed, as it supports embedding HTML and JavaScript, the extensibility of the platform is substantial.

The defacto solution is to use a github repository to provide version control of the materials and to then use that same repository as a hosting solution for the markdown files.
All that is required is to point the main LiaScript site to the URL of the markdown file which is then parsed and rendered as an interactive page.
If materials are commercially sensitive, as with some of the WMG materials, the markdown files can be converted to HTML or SCORM formats using the LiaScript exporter for internal hosting.


## Markdown based

Markdown is a simple markup language.

- Markup language is something like HTML.
  - Text based system to describe structure and formatting.
- Markdown is an especially lightweight system.

-------------------------------------

          {{1-2}}
************************************

Markdown lets you do things like...

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
```markdown
**Bold**, 
*italics*, 
~~underline~~ 
and ~strikethrough~.
```
</div>
<div class="flex-child" style="min-width: 250px">
**Bold**, *italics*, ~~underline~~ and ~strikethrough~.
</div>
</section>

************************************

          {{2-3}}
************************************

Or Maths.

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
```markdown
$$c = \sqrt{a^2 + b^2}$$
```
</div>
<div class="flex-child" style="min-width: 250px">
$$c = \sqrt{a^2 + b^2}$$
</div>
</section>

************************************

          {{3-4}}
************************************

Or Code.

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
````markdown
```cpp
float c = sqrt(pow(a,2) + pow(b,2));
```
````
</div>
<div class="flex-child" style="min-width: 250px">
```cpp
float c = sqrt(pow(a,2) + pow(b,2));
```
</div>
</section>

************************************

          {{4-5}}
************************************

Or Tables:

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
```markdown
| Col A | Col B |
|-------|-------|
| A1    | B1    |
| A2    | B2    |
```
</div>
<div class="flex-child" style="min-width: 250px">
| Col A | Col B |
|-------|-------|
| A1    | B1    |
| A2    | B2    |
</div>
</section>

************************************


          {{5-6}}
************************************

Or Images:

```markdown
![Alt text](https://tinyurl.com/4w84c8dc "Caption")
```

![Alt text](https://tinyurl.com/4w84c8dc "Caption")

************************************


          {{6-7}}
************************************

Or (in LiaScript) quizzes:

<section class="flex-container">
<div class="flex-child" style="min-width: 700px">
```markdown
**If the answer is 42, what is the question?**

[( )] How many roads must a man walk down?
[(X)] What do you get if you multiply six by nine?
[( )] How many towels should you bring?
*********************************************
According to Douglas Adams at least.
*********************************************
```
</div>
<div class="flex-child" style="min-width: 200px">
**If the answer is 42, what is the question?**

[( )] How many roads must a man walk down?
[(X)] What do you get if you multiply six by nine?
[( )] How many towels should you bring?
*********************************************
According to Douglas Adams at least.
*********************************************
</div>
</section>

************************************


          {{7-8}}
************************************

Plus:

- Videos.
- Animations.
- Hyperlinks.
- Footnotes.
- Audio.
- Lists.
- Charts.
- Ascii art.
- TTS.
- And more.

************************************

## Publishing

**Simple option**

- Put the file on Github and point LiaScript site at your file.

**Also pretty simple options**

- LiaScript exporter to output to other formats:

  - PDF.
  - HTML.
  - SCORM (i.e. Moodle).

<iframe src="https://liascript.github.io/">
</iframe>


# Usage in WMG

Multiple courses and modules at various levels.

- UG, MSc, DA, CPD.

- Including online courses.
  - Used commercial platforms for materials creation.
    - High cost, lack of flexibility.
  - Major issue was lack of collaborative content creation.
    - No version control.
    - Collaborative editing was difficult.
    - Repeated incidents of content loss.

## Benefits

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
Because LiaScript is just markdown and therefore a text file.

- Easily version controlled with standard tools.
  - E.g. Git, Mercurial etc.
- Multiple collaborators can work on the same file.
  - Merge versions.
- Changes are tracked.
  - Previous versions can be restored.
  - Very hard to loose work.
</div>
<div class="flex-child" style="min-width: 250px">
![Example of a git commit showing changes](media/git_diff.png "Git showing changes")
</div>
</section>

## Extensions

LiaScript can be extended with custom macros to interface with external tools and APIs.

- Worth noting that the base LiaScript functionality covered 99% our initial use case.
  - CPD course migration.
  - A few HTML iframes to import existing drag-and-drop quizzes.


### Programming

For example, the Programming for AI module:

- Makes heavy use of ability to embed runnable code.
  - Coderunner extension.

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
````markdown
```python
for i in range(5):
  print( f"Hello World! {i}" )
```
@LIA.python()
````
</div>
<div class="flex-child" style="min-width: 250px">
```python
for i in range(5):
  print( f"Hello World! {i}" )
```
@LIA.python()
</div>
</section>


### Cybersecurity

- SQL injection demonstration.
- Uses AlaSQL to run a SQL database in the browser.
  - Based on work by the [DART Team](dart@chop.edu).
- No need for a real database.
  - Multiple students can work at once.

          {{1}}
************************************

@LoginExample

************************************



### Arduino interface

But this was all based on existing extensions.

- Our first original extension was an Arduino interface.
  - Webserial to Arduino.
- Read sensor data or control actuators directly from our learning materials.

-----------------------------------

              {{1}}
************************************

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
Robot arm interface
===================

- Rapid prototyping module.

</div>
<div class="flex-child" style="min-width: 250px">
![](media/PXL_20260624_080228454.jpg)
</div>
</section>

-------------------------------------

************************************


              {{2}}
************************************

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
CAN bus interface
=================

- Automotive Cybersecurity CPD.

</div>
<div class="flex-child" style="min-width: 250px">
![](media/PXL_20250609_093926667.jpg.webp "CAN bus setup in the lab")
</div>
</section>

-------------------------------------

************************************

# CAN hacking

Setup an actual CAN bus using Arduino boards and send real CAN frames between them.

<section class="flex-container">

<div class="flex-child" style="min-width: 250px;">

- Could use 'real' software.
  - License requirements.
  - IT involvement.
  - Need to teach the software.
- Embedding in LiaScript.
  - Free.
  - No deployment issues.
  - Just does what we need.

</div>

<!-- class="flex-child" style="min-width: 600px;" -->
```ascii
             .-------------------. .-------------------.
+--------+   |      +--------+   | |      +--------+   |
|        |   +-.    |        |   | |      |        |   +-.  
|   CANL o <-+ |    |   CANL o <-.-.      |   CANL o <-+ |
|        |     #    |        |            |        |     #
|        |     #    |        |            |        |     # Resistor
|        |     #    |        |            |        |     #
|   CANH * <-+ |    |   CANH * <-.-.      |   CANH * <-+ |
|USB     |   +-.    |USB     |   | |      |USB     |   +-.
+-#------+   |      +-#------+   | |      +-#------+   |
  |          |        |          | |        |          |
 💻👩        |       💻😈        | |       💻👨        |
             .-------------------. .-------------------.
```  

</section>


## Remote open days

WMG has been running virtual open days.

- Increase accessibility / wider audience, 
- In 2025 SCAV course team decided to up our game.
  - Not just the usual virtual lecture.
  - Interactive lab session.
- CAN hacking looks like a bad fit.
  - But! LiaScript classroom functionality.
  - Quickly adjust existing activity.

-------------------------------------------------

          {{1}}
*****************************************

Benefits 

- Student's just need a URL.
  - No accounts, no software installation.
- Collaborative development.
  - Needed to do a fast turn around.

-------------------------------------------------

*****************************************

          {{2}}
*****************************************          
Groups of 2.

- Alice and Charlie.
- Activity takes them through sniffing CAN frames.
  - Then a replay attack, Charlie hacking Alice.
- Same CAN data as with the Arduinos.
  - But sent over the net to the virtual attendees.
*****************************************  

## Alice 👩

@Classroom.defaultManager

<section class="flex-container">
<div class="flex-child" style="min-width: 200px; max-width: 50%;">
@can.alice
</div>
<div class="flex-child" style="min-width: 560px;">
@Dashboard.display
</div>
</section>

### Charlie 😈

@Classroom.defaultManager

@can.retransmit


# Conclusions

- Could achieve all this without LiaScript.
  - *Just* develop a custom websites by teaching all your staff web development.
  - Host your own webserver with account management.

- Substantially easier within LiaScript.

  - Common.

- If using the 'standard' functionality
  - Barrier for entry very low.
  - Markdown is easy.
- Deployment is trivial.
  - Commit and push to Github.
  - Or use LiaEdit, has export button (currently not working?).

- Creating/customisation of extensions is more difficult.
  - HTML and Javascript knowledge.
  - But once created, macro system allows easy reuse.

### Future work

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
Integrating ROS into LiaScript.

- RobotWebTools/Roslibjs for interaction with ROS.
  - Have used them previously to create web interfaces for robots.
- Control is easy.
- Sensor visualisation is proving tricky.
</div>
<div class="flex-child" style="min-width: 250px">
![](media/jazzy.png)
</div>
</section>
