---
layout: guides
navbar: Guides
title: Importing Eclipse Projects from GitHub
key: 3
bump: true

#tags:
#  - text: 'New'
#    type: 'is-primary'
---

Follow these steps to import an Eclipse project from your GitHub repository. You must have completed the steps in the following guides **before** starting this one:

  - [Java and Eclipse Setup](/guides/eclipse/java-and-eclipse-setup.html)
  - [Getting Started with GitHub](/guides/general/getting-started-with-github.html)

It is also recommended to complete the  [Configuring Eclipse](/guides/eclipse/configuring-eclipse.html) guide before this one.

<i class="fas fa-info-circle"></i>
There might be slight differences with the images below at the latest version of Eclipse, and the URLs may have changed to reflect the current term.
{: .notification }

## Import Project into Eclipse (New Repository)
{: .page-header}

Follow these steps if you do not yet have the GitHub repository on your system locally. This includes the *first* time you import the lecture code, homework assignments, and *first* time you import the project.

Log into GitHub, open Eclipse, and follow these steps:

  1. Copy the HTTPS or SSH link to clone your repository in GitHub (based on what you configured earlier):

      ![Screenshot]({{ "/images/eclipse/github-clone-or-download.png" | relative_url }}){: .is-400 }

  1. Switch to Eclipse and select "File" &raquo; "Import..." from the menu. This will bring up this dialog:

      ![Screenshot]({{ "/images/eclipse/eclipse-import-dialog.png" | relative_url }}){: .is-400 }

      Open the "Git" node, select "Projects from Git", and click the "Next" button.

  1. On the next window, select "Clone URI" and click the "Next" button again.

      ![Screenshot]({{ "/images/eclipse/eclipse-import-source.png" | relative_url }}){: .is-400 }

  1. Paste the clone URI copied from GitHub in the "URI" field. The rest should automatically fill in. Click the "Next" button.

      ![Screenshot]({{ "/images/eclipse/eclipse-import-uri.png" | relative_url }}){: .is-400 }

  1. On the next "Branch Selection" screen, keep everything as-is and click the "Next" button again.

      ![Screenshot]({{ "/images/eclipse/eclipse-import-branches.png" | relative_url }}){: .is-400 }

  1. On the "Local Destination" screen, **make sure the path listed for the "Directory" is NOT inside your Eclipse workspace folder**. Click the "Next" button to continue.

      ![Screenshot]({{ "/images/eclipse/eclipse-import-destination.png" | relative_url }}){: .is-400 }

  1. Select the "Import existing Eclipse projects" when prompted and click the "Next" button again.

      ![Screenshot]({{ "/images/eclipse/eclipse-import-wizard.png" | relative_url }}){: .is-400 }

  1. Finally, select the project(s) you want to import. Click "Finish" to complete the import.

      ![Screenshot]({{ "/images/eclipse/eclipse-import-projects.png" | relative_url }}){: .is-400 }

You only need to add this repository once, even if there are multiple Eclipse projects within that repository. For example, you only need to import the [lectures]({{ site.data.info.links.github.link }}/lectures) repository once. However, since each homework assignment has its own repository, you will have to import each repository separately.

## Import Project into Eclipse (Existing Repository)
{: .page-header}

If you already have the repository on your system locally, double-check that you have the latest version of the remote repository by performing a "Pull" action. You can do this in Eclipse [(Help)](http://wiki.eclipse.org/EGit/User_Guide#Pulling_New_Changes_from_Upstream_Branch), via the command-line, or any other `git` tool.

Then, open the import dialog as before (step 2). Select "Existing local repository" in step 3 instead of "Clone URI" and click "Next".

![Screenshot]({{ "/images/eclipse/eclipse-import-select-repo.png" | relative_url }}){: .is-400 }

If the repository is already listed, make sure it is selected. Otherwise, click "Add" to add the repository and follow the prompts. Click "Next" to continue when done.

This will then skip to the wizard in step 7. Follow the prompts as before.

## Troubleshooting
{: .page-header}

If you notice several errors, don't panic! It takes some time for everything to update in Eclipse when you first import a new project. If you notice a progress bar in the lower-right corner, wait for it to finish first.

![Screenshot]({{ "/images/eclipse/eclipse-updating-maven-progress.png" | relative_url }}){: .is-400 }

If the errors do not go away, try these steps first:

  - Update the [maven](https://maven.apache.org/) project by right-clicking the project in the Package Explorer view, selecting "Maven" » "Update Project..." from the menu.

      ![Screenshot]({{ "/images/eclipse/eclipse-update-maven.png" | relative_url }}){: .is-400 }

  - Clean up the project by going to "Project" » "Clean..." in the main menu.

If those steps do not fix it, it is likely something isn't quite right with the build path setup for your Eclipse project. This often happens when your setup is different from expected (either you have a different version of Eclipse or Java).

To fix this, right-click your project folder and select "Build Path" and "Configure Build Path" from the menus. On the following window, click the "Libraries" tab. Remove everything that has a red "x" icon or is listed as "unbound" in the label.

Now, we have to re-add the necessary libraries. Click the "Add Library..." button and follow these steps:

  - Select "JRE System Library" to re-add Java 17 (if necessary). I recommend you select "Workspace default JRE" unless the default is NOT Java 17.

  - Select "JUnit" to re-add JUnit 5 (if necessary). Make sure "JUnit 5" is selected in the dropdown, **not** JUnit 4.

  - ~~Select "User Library" to add a third-party user library (if necessary).~~ This is now handled using the [maven](https://maven.apache.org/) tool.

Click "Apply" and "OK" when done. You should see all of the scary red icons disappear. If not, ask on Piazza or during office hours for help!

If the Eclipse workspace becomes corrupted somehow, it is often easier to make sure your code is committed and pushed to GitHub, and then re-importing everything in a fresh workspace.
