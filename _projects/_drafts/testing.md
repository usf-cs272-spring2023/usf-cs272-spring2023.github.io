---
title: Project Testing
navbar: Resources
layout: resources
key: 0.2

tags:
 - text: 'Pending'

---

<i class="fas fa-video"></i>
There is a 33 minute [walkthrough video](https://drive.google.com/file/d/13JeruWvyf6d2wXbXxGoXFGbM10uL8UD1/view?usp=sharing) an overview of projects, how to test projects locally (starting at 13:53), testing projects remotely (starting at 24:33), and get your first functionality grade (starting at 29:38). However, it is for an earlier semester and some parts of the process may now look different.
{: .notification .is-success }

You must verify your project is passing the functionality tests to earn the project tests grade as well as before every code review required to earn your project final release grade. This guide walks through that testing process.

This guide assumes you have already [setup your project](setup.html) in Eclipse.

## Testing Locally
{: .page-header }

It is important to work and test the projects iteratively, as well as attempt to pass the tests locally before considering running the tests remotely.

### Running JUnit Tests

You must use the [JUnit 5](https://junit.org/junit5/) tests provided with the [project-tests]({{ site.data.info.links.github.link }}/project-tests) repository to determine if your project is meeting the required functionality. The suite of tests for each project are given by the `*Tests.java` files in the [src]({{ site.data.info.links.github.link }}/project-tests/tree/main/src/test/java/edu/usfca/cs272) subdirectory. For example, the `v1.0.0` tests for [Project 1](project-1.html) are provided by the [BuildCountTests.java]({{ site.data.info.links.github.link }}/project-tests/blob/main/src/test/java/edu/usfca/cs272/BuildCountTests.java) file.

While you are initially working on the project, **focus on individual tests** or individual groups of tests. There is no need to run all of the tests every time; in fact this can make debugging harder! See the [Homework Testing](/resources/homework/test-homework.html#running-individual-tests) guide for different ways you can run individual JUnit tests. Here is an example run configuration:

![Screenshot]({{ "/images/projects/project-eclipse-junit-run-configuration.png" | relative_url }}){: .is-600 }

**Pay attention to the test output.** The output tells you where to find the actual and expected output files and what caused the test to fail. It also tells you the arguments that were passed to your `Driver` class, which is helpful for debugging. The full output, which can be copied to the console for easy copying/pasting using the <img src="{{ "eclipse-copy-junit-output-to-console.png" | prepend: "/images/projects/" | relative_url }}" style="height: 16px; vertical-align: middle;"> button, includes the following text:

```
Actual File:
    actual/index-simple-hello.json
Expected File:
    expected/index/index-simple/index-simple-hello.json
Arguments:
    -text input/text/simple/hello.txt -index actual/index-simple-hello.json
Message:
    Difference detected on line: 2.
```

This gives the actual arguments passed to `Driver` by the test, the actual and expected files being compared, and where the first difference was detected. You can use the [Compare Editor](http://help.eclipse.org/latest/topic/org.eclipse.platform.doc.user/reference/ref-25.htm?cp=0_4_4_1_2) in Eclipse to compare the files side-by-side for debugging, or use the [Run Configurations](#running-driver) in Eclipse to enter the same arguments manually.

<i class="fas fa-exclamation-triangle"></i>
To save space, the tests automatically delete your output files if they match the expected output. Only output files for failing tests will be kept.
{: .notification .is-danger }

### Running Driver

When you first start, very little template code is provided and the tests provided only test the final output of your project (unlike the homework which has tests for individual methods). Depending on your approach, you may want to pass in your own arguments to the `Driver` class using a "Run Configuration" and manually inspecting the output.

<details>
<p><summary>View Details</summary></p>

<div markdown=1>

  1. Go to the "Run" menu in the menubar at the top of Eclipse and select "Run Configurations..." from the dropdown.

  1. Click on the "Java Application" category in the left view pane and click the "New Configuration" button (looks like a blank file with a +).

  1. Enter any name you'd like in the "Name:" text box.

  1. Next to the "Project:" text box, click the "Browse" button and select the "SearchEngine" project.

  1. Next to the "Main class:" text box, click the "" button and select "Driver" from the list. At this point, your setup should look like this:

      ![Screenshot]({{ "/images/projects/eclipse-project-driver-run-main.png" | relative_url }}){: .is-600 }

  1. Click on the "Arguments" tab. Enter the following into the "Program arguments" text area:

      ```
      -text input/text/simple/hello.txt -index actual/index-simple-hello.json
      ```

      ...or if you are on Windows:

      ```
      -text input\text\simple\hello.txt -index actual\index-simple-hello.json
      ```

      This will run `Driver` with the same arguments as the first project 1 test.

  1. **This next part is really important!** Remember, all of the test files are in a SEPARATE repository. Change the "Working directory:" setting to the "Other:" choice.

  1. Click the "Workspace..." button and select the "SearchEngineTests" project. At this point, your setup should look like this:

      ![Screenshot]({{ "/images/projects/eclipse-project-driver-run-arguments.png" | relative_url }}){: .is-600 }

  1. Click the "Apply" and "Run" buttons.

</div>
</details>

<br/>
You can keep running this same "Run Configuration" while debugging. If you are failing a specific JUnit test, you can copy/paste the arguments used by the test (provided in the failure output) to debug your code.

## Testing Remotely
{: .page-header }

You must test your code remotely before you can earn credit for the functionality of your project and before you can sign up for a code review appointment.

This process begins by creating a release on GitHub. This will trigger the GitHub Action that verifies your project functionality.

### Creating Releases

Creating releases will familiarize you with [**versioning**](https://en.wikipedia.org/wiki/Software_versioning) your code.

  1. After passing all of the tests locally and pushing your latest commits to GitHub, follow the [Creating Releases](https://help.github.com/articles/creating-releases/) steps to draft a new release of your project code on GitHub.

  2. Choose a "tag" or the version number to assign to the code at this stage. Out in the "real world" you will likely use [semantic versioning](https://semver.org/), which we will roughly mimic in class.

      Specifically, you must name your release `v#.#.#` where the first `#` is the project number (1, 2, 3, or 4), the second `#` is the number of code reviews you've had for that project, and the last `#` is the number of patches/fixes you released in between code reviews for that project.

      For example, your first release should be `v1.0.0` because it is for project 1, you have not had any code reviews yet, and you have not had any other releases yet. If your code does not pass the tests remotely, then you have to fix your code and re-release your project as `v1.0.1` since you now have 1 prior release. After your first code review, the next release will be `v1.1.0` (notice how the last number reset to 0).

      The release `v2.3.4` means this release is for project 2, you have had 3 code reviews for project 2 so far, and this is the 4th release since your last code review of project 2. (It also means there must be a prior `v2.3.3` release made before this one.)

  3. Enter the tag number in the "Choose a tag" dropdown and click the "Create new tag on publish" option. If you are working ahead and have the project code in a different branch, select that branch in the "Target" dropdown.

      ![Screenshot]({{ "/images/projects/github-create-release.png" | relative_url }}){: .is-400 }

  4. Click the "Publish release" button. You can leave the title and description blank. You *should* click the "This is a pre-release" checkbox unless you have passed code review, but it is not something we enforce currently.

You can see a [sample release]({{ site.data.info.links.github.link }}/project-template/releases) on the template repository.

### Deleting Releases

**Generally, you should not delete releases even if they are not passing tests.** The exception is if you created a release with the wrong version number. In that case, you should follow these steps to delete the release **and** the underlying tag on GitHub.

<details>
<p><summary>View Details</summary></p>

<div markdown=1>

  1. Go to the "Code" section in your repository and then click "Releases" on the right sidebar. Find the release you want to delete and click the <i class="far fa-trash"></i> trash icon to delete the release:
      
      ![Screenshot]({{ "/images/projects/github-release-delete.png" | relative_url }}){: .is-400 }

  2. Click on the "Tags" tab and find the tag that was created for the release. Click the tag title. You should see something like this:

      ![Screenshot]({{ "/images/projects/github-tag-delete.png" | relative_url }}){: .is-400 }

  3. Click on the "Delete" button to delete the tag. This is an important step, otherwise this version number cannot be reused again in the future!

</div>
</details>

<br/>
Again, you should only go through these steps if you create a release with the wrong version number. Otherwise, keep the other releases around for debugging and to track your progression.

### Project Release Action

Creating or editing a release triggers the "Project Release" action in the "Actions" tab to run automatically. It will have the same title as the release tag:

![Screenshot]({{ "/images/projects/actions-run-project-tests.png" | relative_url }}){: .is-600 }

To view details, click on the run title. The yellow circle <i class="fas fa-circle has-text-warning"></i> icon indicates the run or step is still in progress and has not yet completed. The red circle <i class="fas fa-times-circle has-text-danger"></i> indicates the run or step failed (either because of setup issues or the tests failed). The green circle <i class="fas fa-check-circle has-text-success"></i> icon indicates the run or step completed and was successful (i.e. all tests passed).

You can see an [example run]({{ site.data.info.links.github.link }}/project-template/actions/workflows/project-release.yml) in the template repository. You can still request a "Project Tests" grade from this release if the action fails, as long as the "Run / ... / Tests" step passes. In the summary, you will see a messages similar to:

> ✅ The release v1.0.x may be used to request a project 1 tests grade. This grade only needs to be requested once.

If you do not see this message, then the tests did not pass and you will need to debug what went wrong.

## Debugging
{: .page-header }

If the "Project Release" action failed, you need to debug what went wrong. Details on what happened can be found in the detailed view.

### Release Action Run Details

To see the detailed log of everything that happened, click on the job heading that failed. There are three jobs:

  1. **"Run / Check / Tests"**: This job sets up the virtual machine, compiles the Java code, and runs the tests. There are three groups of tests run: tests for the current project, tests for the past project, and tests for the next project (making sure you don't have the next project functionality in the current branch). This job must pass to earn a "Project Tests" grade, request a code review appointment, or earn a "Project Design" grade.

  1. **"Run / Check / Style"**: This job checks that the code compiles without warnings, and does a keyword search for other potential problems (like leftover `TODO` comments or extra `main` methods). This job must pass to request a code review appointment or earn a "Project Design" grade.

  1. **"Run / Save / Results"**: This job makes sure to save the results so that when you request a grade or code review, the bot can figure out whether that release is eligible. If this step fails, something is wrong with either the action or GitHub---not your code. Reach out on Piazza for how to proceed.

Look for the red circle <i class="fas fa-times-circle has-text-danger"></i> icon to figure out which jobs and steps failed, click the greater-than <i class="far fa-angle-right"></i> icon to see the step details, and the triangle or caret <i class="fas fa-caret-right"></i> icon to open details within a step. You will often have to scroll up in the logs to find the start of the errors.

### Running Maven Locally

Most of the time, if you are passing the tests locally you will also pass them remotely. However, there are slight differences between how the tests are run by Eclipse and how they are run by GitHub Actions.

If you want to test out the same approach used by GitHub Actions, you have to use Maven to run the tests instead of the JUnit interface in Eclipse.

<details>
<p><summary>View Details</summary></p>

<div markdown=1>

Here is a screenshot of the run configuration you must create:

![Screenshot]({{ "/images/projects/eclipse-maven-project-configuration.png" | relative_url }}){: .is-600 }

Follow these steps to create this run configuration:

  1. Go to the "Run" menu in the menubar at the top of Eclipse and select "Run Configurations..." from the dropdown.

  1. Click on the "Maven Build" category in the left view pane and click the "New Configuration" button (looks like a blank file with a +).

  1. Enter any name you'd like in the "Name:" text box.

  1. Under the "Base directory:" text box, click the "Workspace" button and select the "SearchEngine" project.

  1. Enter the following into the "Goals:" text box:

      ```
      clean test
      ```

      This combination will remove all previously-compiled class files, re-compile the main code and test code, and run the JUnit tests.

  1. Click the "Add..." button to add a parameter. Enter `groups` and `test-v#` for the name and value, where `#` is the project major and minor version (e.g. `test-v1.0`). This tells Maven to run just the tests that GitHub Actions focuses on for this project.

  1. [OPTIONAL] Click the "Add..." button to add another parameter. Enter `compileOptionFail` and `true` for the name and value. This tells Maven to fail if any compile warning are detected. If there are warnings, it will report those warnings and exit (it will not run the tests). *This is not required to earn project test credit, but will be required to sign up for a code review.*

</div>
</details>

<br/>
These steps are really only required for fine-grained debugging and can be skipped most of the time.
