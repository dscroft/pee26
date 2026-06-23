<!--
module_id: pee26_presentation
author:   David Croft
email:    david.croft@warwick.ac.uk
version: 0.0.1

language: en
narrator: UK English Female

title: LiaScript for real and virtual hardware control
comment:  This is the presentation session for PEE'26
estimated_time_in_minutes: 10

@learning_objectives  
- Be aware of LiaScript
- Learn benefits of text-based systems
- See timeline and pathway to current WMG usage.

@end

@style
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

@version_history 

Previous versions: 

@end

import: https://raw.githubusercontent.com/LiaScript/CodeRunner/master/README.md
import: assets/macros.md
import: assets/sqli_demo.md
-->


# LiaScript for real and virtual hardware control

These slides are created using LiaScript.

<section class="flex-container">
  <div class="flex-child">

    - If want to follow along or explore LiaScript.
  </div>
  
  <div class="flex-child">
    [qr-code](https://liascript.github.io/)
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

          {{0-1}}
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

          {{1-2}}
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

          {{2-3}}
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

          {{3-4}}
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


          {{4-5}}
************************************

Or Images:

```markdown
![Alt text](https://tinyurl.com/4w84c8dc "Caption")
```

![Alt text](https://tinyurl.com/4w84c8dc "Caption")

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

Because LiaScript is just markdown and therefore a text file.

- Easily version controlled with standard tools (e.g. Git).
- Multiple collaborators can work on the same file without conflicts.
- Changes are tracked, and previous versions can be easily restored if needed.

## Extensions

### Programming for AI

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


### Automotive Cybersecurity CPD

SQL injection demonstrations

- Uses AlaSQL to run a SQL database in the browser.
  - Based on work by the [DART Team](dart@chop.edu).

          {{1}}
************************************

@LoginExample

************************************



## Arduino interface

- LiaScript can be extended with custom macros to interface with external tools and APIs.
- For example, we can create a macro to interface with an Arduino board, allowing us to read sensor data or control actuators directly from our learning materials.
- This activity was then used in a practical session where students could interact with the Arduino through the LiaScript interface, providing a hands-on learning experience.


## Remote open days




## Future work

<section class="flex-container">
<div class="flex-child" style="min-width: 250px">
Integrating ROS into LiaScript.

- RobotWebTools/Roslibjs for interaction with ROS.
  - Have used them previously to create web interfaces for robots.
  - Working on a LiaScript wrapper.
</div>
<div class="flex-child" style="min-width: 250px">
![](media/jazzy.png)
</div>
</section>



## Conclusion

- Could achieve this functionality without LiaScript.

  - *Just* develop a custom websites.

- Substantially easier within LiaScript.

  - Common.