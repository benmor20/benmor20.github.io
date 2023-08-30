# Easel: Work with Canvas

During the summer of 2023, I worked with Steve Matsumoto, a professor of Computing at Olin College, to create Easel: a git-like command line tool to make certain Canvas functionality courses easier to use. As the sole researcher on this project, I was responsible for assessing the problem, designing a new workflow, and implementing a solution.

## My Work

I issued a survey to Olin faculty, and based on the results I found a few major difficulties in working with Canvas. For instance, it is not possible to copy a quiz on Canvas. So, if you say, would like to create a self-assessment quiz for each week of the semester, you must do that manually. Or, if you would like to copy a course into a new semester, you must manually update each of the dates, a process which can take hours.

I designed Easel in an attempt to mitigate these problems. I wanted to make Easel to be quick and easy to learn, so I chose to represent Easel courses in Markdown. As eac resource is its own file, this means each file can be copied with ease, and entire courses (groups of files) can be as well. Additionally, I based Easel commands on git commands as they are familiar to those who use applications like this, and the basic commands are easy to pick up for those who are not. I made a specific Markdown format for each type of Canvas resource, so that each resource has one file associated with it. The format is very similar across each type of resource:

```markdown
# [name of resource]

[description of resource]

## Metadata

[list of metadata variables]
```

This is a quick-and-easy way to summarize all the information and settings needed for each resource, so I made it standard for all Easel documents. You can find [the easel repo here](https://github.com/olincollege/easel), along with [the primary branch I worked on](https://github.com/olincollege/easel/tree/basic-structure). See the [README](https://github.com/olincollege/easel/blob/420666e833463b7a1714ed896dbead951464cdc7/README.md) for more in-depth information on what Easel is and how it works.

As I was the first one to work on Easel, I got to create and implement its foundation. I chose to use markdown to represent Canvas resources locally, as it is a widely used, lightweight, and human-readable way to format text. There are many markdown tutorials available for anyone who would be using it for the first time, and, through [pandoc](https://pandoc.org/), it can easily be converted to HTML for easy parsing.

### Proof-of-Concept: Date Transfer

As a proof of concept, I decided to implement a stand-alone utility to transfer dates from one semester to another. This gained me familiarity with the Canvas API, allowing me to alleviate what many professors indicated was a major pain point: updating assignment due dates for a new semester. You can find my work on the [date-transfer branch of the Easel repo](https://github.com/olincollege/easel/tree/date-transfer).

To do this, I created a JSON representation of a generalized Academic Calendar using ISO-8601 standardized datetime strings. I also extended this standard for the purposes of this academic calendar to allow for specifying a time within a "cycle" (say, a week, or a month, or day 2 of a 3 day cycle) to allow for the calendar to be as general as possible.

### Easel

After my proof of concept, I started work on Easel. I created an `EaselDoc` class to represent each markdown document, including functions for parsing, standardizing, uploading, and downloading resources. I implemented three commands over the course of the summer: `easel create`, `easel push`, and `easel pull`, giving basic functionality to Easel. Finally, I created the ground work to [represent a certain date and time within a semester](https://github.com/olincollege/easel/blob/420666e833463b7a1714ed896dbead951464cdc7/todo.md#due-dates), so that specific datetime strings do not need to be used.

<br>

*easel in Easel picture from Ciprian Popescu on The Noun Project*
