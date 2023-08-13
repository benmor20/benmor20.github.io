# Easel: Work with Canvas

During the summer of 2023, I worked with Steve Matsumoto, a professor at Olin College, to create Easel: a git-like command line tool to make certain aspects of working with Canvas courses a bit easier. For instance, it is not possible to copy a quiz on Canvas. So, if you say, would like to create a self-assessment quiz for each week of the semester, you must do that all manually. Or, if you would like to copy a course into a new semester, you must manually update each of the dates, a process which can take hours.

Easel attempts to mitigate these problems. Easel courses are represented in Markdown, and each course can be uploaded to and downloaded from Canvas with simple, git-like commands through the use of the Canvas API. You can find [the easel repo here](https://github.com/olincollege/easel), along with [the primary branch I worked on](https://github.com/olincollege/easel/tree/basic-structure). See the [README](https://github.com/olincollege/easel/blob/420666e833463b7a1714ed896dbead951464cdc7/README.md) for more in-depth information on what Easel is and how it works.

## My Work

As I was the first one to work on Easel, I got to create and implement its foundation. I chose to use markdown to represent Canvas resources locally, as it is a widely used, lightweight, and human readable way to format text. There are many markdown tutorials available for any people who would be using it for the first time, and, through [pandoc](https://pandoc.org/), it can easily be converted to HTML for easy parsing. For interfacing with Easel, I chose to make it a command-line utility largely due to time constraints for the time I had to work with the project.

### Proof-of-Concept: Date Transfer

As a proof of concept, I decided to implement a stand-alone utility to transfer dates from one semester to another. This gained me familiarity with the Canvas API and would allow me to allieviate what many professors indicated was a major pain point: updating assignment due dates for a new semester. You can find my work on the [date-transfer branch of the Easel repo](https://github.com/olincollege/easel/tree/date-transfer).

To do this, I created a JSON representation of a generalized Academic Calendar using ISO-8601 standardized datetime strings. I also extended this standard for the purposes of this academic calendar to allow for specifying a specific time within a "cycle" (say, a week, or a month, or day 2 of a 3 day cycle) to allow for the calendar to be as general as possible. With this, I was able to:

1. Load a current and previous academic calendar
2. Read each due date from every assignment in a course
3. Convert each due date from the old semester to the new semester (assuming a system of "x days from the nth class")
4. Update the due dates on Canvas

### Easel

After my proof of concept, I started work on Easel. I created an `EaselDoc` class to represent each markdown document, including functions for parsing, standardizing, uploading, and downloading resources. I implemented three commands over the course of the summer: `easel create`, `easel push`, and `easel pull`, giving basic functionality to Easel. Finally, I created the ground work to [represent a certain date and time within a semester](https://github.com/olincollege/easel/blob/420666e833463b7a1714ed896dbead951464cdc7/todo.md#due-dates), so that specific datetime strings do not need to be used.
