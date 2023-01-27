---
layout: resources
navbar: Resources
title: Completing Homework
key: 2

example_issue: 'https://github.com/usf-cs272-spring2023/homework-ArgumentParser-template/issues/2'
---

This guides discusses how to work on your homework and submit it for grading.

## Modifying Homework Code
{: .page-header }

Refer to the `TODO` comments in the template code for specifics on what code to fill in or modify for the assignment. When modifying this code, keep in mind the following:

  - **Do not modify any method or class declarations.** For example, you cannot add or remove keywords like `static` or add a `throws` keyword to a method if one was not already included.

  - **Do not modify (non-main) methods that are already fully defined.** For example, if a method is provided and has no `TODO` comments within the method definition, you may not modify that method.

  - **Do not modify the test code.** This includes any input or output files utilized by the test code.

  - You MAY add additional members and methods as needed to the code.

  - You MAY add additional classes and imports for those classes as long as they are a part of core Java or one of the pre-approved third-party libraries (Apache OpenNLP, Apache Commons Text, Apache Log4j2, Jetty, MariaDB).

  - You MAY add comments to clarify code for yourself. You can add comments to already defined methods if you want.

  - You MAY change a `return` statement in a method with a `TODO` comment as needed. (You may *not* change the return type in the method declaration, however.)

  - You MAY remove the `main` method if you do not need it for debugging. 

  - You MAY remove the `TODO` comments when you are done. This removes them from the "Tasks" view in Eclipse. If you want to keep the comment around, you can change the text `TODO` to `DONE` instead.

Post on [Piazza]({{ site.data.info.links.forums.href }}) if you have any questions regarding what must be done for a homework assignment or a question about your homework grade.

## Submitting On-Time Homework
{: .page-header }

<i class="fas fa-video"></i>
Our TA has recorded a [super helpful walkthrough](https://usfca.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=1b49784b-448a-46b7-a32a-aeff00129c93) if you prefer a video of these steps! You may need to login with your USF username and password for access.
{: .notification .is-success }

To receive full credit on any homework assignment, you must:

  - **Pass all of the unit tests remotely.** See the [Testing Homework](/resources/homework/test-homework.html) guide detailed steps. You will not receive full points if you are passing the tests locally but not remotely.

  - **Commit your work often.** Some assignments require a minimum number of commits! Consider making a commit after you complete every `TODO` in the code or after you pass a new test. <i class="fas fa-star has-text-warning"></i>

  - **Avoid warnings.** Some assignments require the code to compile with no warnings to earn full points. Try to make sure your code is warning-free in Eclipse before testing your code remotely!

  - **Follow the `TODO` directions.** For example, if the `TODO` comment stated you must use a `HashSet` and your code does not, you could lose points even if you are passing all of the unit tests.

  - **Properly submit your homework on time.** By default, GitHub Classroom will report your grade based on the last commit *before* the deadline and will ignore all other commits after that.

To submit your homework, you must make a commit and push that commit to GitHub before the deadline!

## Submitting Late Homework
{: .page-header }

<i class="fas fa-video"></i>
Our TA has recorded a [super helpful walkthrough](https://usfca.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=7da811a2-478f-4b9f-8c47-aeff00160d09) if you prefer a video of these steps! You may need to login with your USF username and password for access.
{: .notification .is-success }

GitHub Classroom will report your grade based on the last commit pushed **before** the deadline and will ignore all commits pushed after that. To have your homework regraded after the deadline, you must:

  1. Make sure you are happy with the late grade reported by the most recent GitHub Action run. Pay attention to the grade reported by the "Check Autograding Results" annotation.
      
      ![Screenshot]({{ "/images/homework/annotation-late.png" | relative_url }}){: .is-300 }

  2. Create an issue using the "Late Homework Request" template. Follow the instructions to fill in your name and USF username.

      ![Screenshot]({{ "/images/homework/late-issue-template.png" | relative_url }}){: .is-300 }

  3. Wait for the GitHub Action bot to run. Within a few minutes, it should respond with:
  
      - If there are problems with your request, the bot will report the problems and close the issue. You must fix those problems and then re-open the issue to trigger it to run again.

          ![Screenshot]({{ "/images/homework/late-issue-error.png" | relative_url }}){: .is-400 }

      - If there are no problems with your request, the bot will respond with the grade that should be entered into Canvas and assign the issue to the teacher assistants to process. Once they have entered your grade, they will close the issue.

          ![Screenshot]({{ "/images/homework/late-issue-success.png" | relative_url }}){: .is-400 }
  
      If you are uncertain what to do or your issue has been open for over 2 business days without any activity, reach out on [Piazza]({{ site.data.info.links.forums.link }}) for help.

See [this example issue]({{ page.example_issue }}) for what the request should look like when it runs properly.