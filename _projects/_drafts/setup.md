---
title: Project Setup
navbar: Resources
layout: resources
key: 0.1

tags:
 - text: 'Pending'
---

<i class="fas fa-video"></i>
There is a [walkthrough video](https://drive.google.com/file/d/14oecs2WIJrRQJnWnRlKE2-4nt2lZa5vW/view?usp=sharing) of these steps, but it is for an older version of Java and tests. The process, however, is the same! 
{: .notification .is-success }

The project is broken into two separate GitHub repositories and Eclipse projects:

  - **SearchEngine**: This is where you will place your project code. You will have your own private version of this project. The initial template includes some configuration files and a skeleton `Driver.java` class. The repo will be named `project-[username]` where `[username]` is your GitHub username (not your USF username).

  - **SearchEngineTest**: This is where the project tests files and expected output files are located. Everyone shares the same read-only repository. It will start with the [Project 1](project-1.html) test files first and will be updated throughout the semester with new test files for the other projects. The repo will be named [`project-tests`]({{ site.data.info.links.github.link }}/project-tests) for everyone.

You will reuse these same repositories for all of the projects.

When we verify the functionality and review the design of your project, we will only checkout the **SearchEngine** repository each time. This helps avoid having to re-copy the large test files over and over again.

## Create and Import Projects

Below is a quick summary of the **one-time setup** needed for the project:

  1. Visit the ~~[Project Setup](#)~~ assignment in Canvas and click the GitHub Classroom link. This will setup a private repository named `project-username` where `username` is your GitHub username. You will use this same repository the *entire* semester for all of the projects.

  1. Import the repository as a Java Project in Eclipse. See the [Importing Eclipse Projects from GitHub](/resources/eclipse/importing-eclipse-projects-from-github.html) guide for steps. This will create a "SearchEngine" project in Eclipse where you will add your own code for the project.

  1. Import the `project-tests` repository at <{{ site.data.info.links.github.link }}/project-tests> as a Java Project in Eclipse. This will create a "SearchEngineTests" project in Eclipse where you will find all of the test code and data files.

      <i class="fas fa-exclaimation-triangle"></i>&nbsp;**These two projects must be located in the same parent directory!** For example `cs272_github/project-username` and `cs272_github/project-tests` share the same parent directory `cs272_github`.

  1. If there are compile issues, right-click the "SearchEngine" project in your "Package Explorer" view, go to the "Maven" menu, and select "Update Project..." from the submenu. Make sure "SearchEngine" is selected and click the "OK" button.

After importing into Eclipse, the "Package Explorer" view should look *similar* to this:

![Screenshot]({{ "/images/projects/project-eclipse.png" | relative_url }}){: style="width: 250px;" class="is-size is-bordered" }

Important files or directories are highlighted in blue. *(Your view may not have this highlighting.)* You may also be missing the `actual` folder until your project is producing output files.

**Once setup, you do not need to go through these steps again.**

## Verify Setup

Once you have everything imported into Eclipse, try these steps to verify everything is setup correctly:

  1. Verify you can run the `BuildCountTests.java` set of tests in the "SearchEngineTests" project in Eclipse.

  1. Verify you can make, commit, and push changes to `Driver.java` in the "SearchEngine" project in Eclipse.

  1. Create your first release. Click the "Releases" heading (usually on the right side) in GitHub and click the "Draft a new release" button. Enter `v1.0.0` as the tag version, *optionally* click the "This is a pre-release" checkbox, and leave the other fields unchanged:

      ![Screenshot]({{ "/images/projects/github-create-release.png" | relative_url }}){: .is-400 }

      There is an [example release]({{ site.data.info.links.github.link }}/project-template/releases/tag/v1.0.0) in the `project-template` repository.

      <i class="fas fa-info-circle"></i>
      In practice, we use [releases](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases) and [semantic versioning](https://semver.org/) to deploy of software to its users. In this class, we will use releases and semantic versioning to (1) determine which project (and at what stage) you are submitting, and (2) make sure you have experience working with both of these practices before graduating. These are discussed further in the [Project Testing](testing.html) guide.
      {: .notification .is-size-6 } 

  1. Go to the "Actions" tab and make sure the "Project Release" workflow ran for the `v1.0.0` release. There is an [example run]({{ site.data.info.links.github.link }}/project-template/actions/workflows/project-release.yml) in the `project-template` repository. It *should* fail, since you don't have any code yet.

If you are able to complete all of the above, you should be ready to start your project! More about releases and the workflow output can be found in the [Project Testing](testing.html) guide.

## Folder Structure

Here is the folder structure for the project and its tests:

```
├── SearchEngine (project-username)
│   ├── pom.xml (should not need modification)
│   ├── src/main/java/edu/usfca/cs272
│   │   ├── Driver.java (modify as needed)
│   │   └── (add other *.java files here)
│   └── src/main/resources
│       └── log4j2.xml (optional)
└── SearchEngineTest (project-tests)
    |── pom.xml (should not need modification)
    ├── src/test/java/edu/usfca/cs272
    │   ├── BuildCountTests.java
    │   └── (other test files here)
    ├── expected
    │   ├── index
    │   │   └── (expected index-*.json files)
    │   └── (other directories for future projects)
    ├── input
    │   ├── text
    │   │   └── (text files to use as input)
    │   └── (other directories for future projects)
    └── actual
        └── (output files generated by your code)
```

If you run a test from the **SearchEngineTest** project, the output files will appear in the **SearchEngineTest** project even though your code is in the **SearchEngine** project.

If you run the `main` method via a Runtime Configuration in Eclipse from the **SearchEngine** project, then your output files will appear in the **SearchEngine** project.
