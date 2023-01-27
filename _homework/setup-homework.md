---
layout: resources
navbar: Resources
title: Setup Homework
key: 1

run_summary: 'https://github.com/usf-cs272-spring2023/homework-ArgumentParser-template/actions/runs/4026248931'
run_detail: 'https://github.com/usf-cs272-spring2023/homework-ArgumentParser-template/actions/runs/4026248931/jobs/6920518568#step:3:91'

tags:
 - text: 'New'
   type: 'is-primary'

---

This guide will walk you through the process of setting up your homework repository and importing it into Eclipse.

<i class="fas fa-video"></i>
Our TA has recorded a [super helpful walkthrough](https://usfca.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=1b49784b-448a-46b7-a32a-aeff00129c93) if you prefer a video of these steps! You may need to login with your USF username and password for access.
{: .notification .is-success }

## One-Time Setup
{: .page-header }

Before you begin, you need to make sure to go through the other setup guides first:

  - [General Guides](/resources/general/)
  - [Eclipse Guides](/resources/eclipse/)

This includes making sure you have a GitHub account linked to your USF email. See the [Getting Started with GitHub](/resources/general/getting-started-with-github.html) guide for details.

## Per-Homework Setup
{: .page-header }

For every homework assignment, you will need to repeat these steps:

  1. **Setup your GitHub repository.**

      1. Find the homework assignment on Canvas. For example, go to the [{{ site.data.homework.argumentparser.text }}]({{ site.data.homework.argumentparser.link }}) assignment on Canvas for the first homework of the semester.

      1. Click the GitHub Classroom invitation link and follow the instructions.

      1. If this is your first time using GitHub Classroom, you will be prompted to select your USF username from the list. This makes sure we can associate your GitHub username with your grades in Canvas. You only need to complete this step once.

      This process will create a private GitHub repository with the homework starter code that only you, the teacher assistants, and the instructor can access.

  1. **Setup your Eclipse project.**

      Once your repository is setup, you must import the homework as a Java Maven Project in Eclipse. See the [Importing Eclipse Projects from GitHub](/resources/eclipse/importing-eclipse-projects-from-github.html) guide for detailed steps.

      When done, the "Package Explorer" view of your homework should look something like this:

      ![Screenshot]({{ "/images/homework/homework-imported.png" | relative_url }}){: .is-300 }

      Make sure the correct Java version is showing up and that you are in the "Java" perspective before moving on. This perspective allows you to run Java code. The "Git" perspective lets you view, but not run, Java code.

  1. **Verify you can run the JUnit tests on Eclipse locally.**

      The tests will be in the `src` » `test` » `java` subfolder. Right-click the test file and select "Run As" » "JUnit Test" from the menu.

      That should open up the "JUnit" view automatically, which will look similar to this:

      ![Screenshot]({{ "/images/homework/homework-junit.png" | relative_url }}){: .is-300 }

      These are the tests you will need to pass for the homework. Most of the tests will fail the first time.

  1. **Commit and push changes to GitHub.**

      In Eclipse, make a minor change and [commit and push](http://wiki.eclipse.org/EGit/User_Guide#Committing_Changes) that change to GitHub.

      I recommend placing the [Git Staging View](http://wiki.eclipse.org/EGit/User_Guide#Git_Staging_View) somewhere on your Eclipse layout to make this process more convenient. You can access that view via the "Window" » "Show View" » "Other..." option in the menu. Open the "Git" folder to find the "Git Staging View" option to add it to your layout:

      ![Screenshot]({{ "/images/homework/eclipse-show-view.png" | relative_url }}){: .is-300 }

      Make sure to drag files from the "Unstaged" to the "Staged" box and add a commit message. Sometimes, you need to click the "Refresh" <i class="far fa-sync"></i> button in the upper right corner of the view to update. For example:

      ![Screenshot]({{ "/images/homework/homework-staging.png" | relative_url }}){: .is-600 }

      Click "Commit and Push" to make a checkpoint and push that checkpoint to GitHub. If you click "Commit" you only make a local checkpoint on your system.

  1. **Verify the Autograder GitHub Action ran properly.**

      Visit your repository on GitHub. Click the "Actions" tab to view all of the runs. There should be a run with the same name as your last commit.

      Make sure the action finishes running. Here is an example of what a completed action looks like:

      ![Screenshot]({{ "/images/homework/github-homework-action-summary.png" | relative_url }}){: .is-600 }  
      <{{ page.run_summary }}>

      Scroll down in the "Summary" view to see the "Annotations" section. That will report the number of points your homework has earned so far. The first annotation named "Autograding complete" shows the points returned by the GitHub Classroom Autograder. The second annotation mamed "Check Autograding Results" takes into account late penalties (if appropriate):

      ![Screenshot]({{ "/images/homework/homework-annotation.png" | relative_url }}){: .is-300 }

      For details on which tests failed, you need to drill into the "Autograding" output.

      ![Screenshot]({{ "/images/homework/github-homework-action-detail.png" | relative_url }}){: .is-400 }  
      <{{ page.run_detail }}>

      You can also see the number of points on the README.md file as well:

      ![Screenshot]({{ "/images/homework/homework-readme.png" | relative_url }}){: .is-400 }

      You may need to refresh your view on GitHub to see the latest result.

At this point, you should be able to start [completing](complete-homework.html) and [testing](test-homework.html) the homework.
