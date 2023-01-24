---
layout: resources
navbar: Resources
title: Configuring Eclipse
key: 2

---

Eclipse is a free, powerful, open-source, and configurable IDE. I recommend you spend some time configuring Eclipse to meet your needs.

## Compiler Configuration

The homework and project code should not have warnings, including Javadoc warnings. If you want your Eclipse to have *similar* compiler settings, import this configuration into your Eclipse workspace:

<figure>
<script src="https://gist.github.com/sjengle/c7d572a4d0eaf0be618f95d761d49a08.js"></script>
<caption><a href="https://gist.github.com/sjengle/c7d572a4d0eaf0be618f95d761d49a08">https://gist.github.com/sjengle/c7d572a4d0eaf0be618f95d761d49a08</a></caption>
</figure>

To use the above configuration, scroll to the "view raw" link and open it in a new window. Save the output as `eclipse-compiler-settings.epf` to your system. (Alternatively, copy and paste the text into a file with this name.)

In the Eclipse Preferences view, click the "Import" button near the bottom left and select the `epf` file. Or, [follow these steps](https://help.eclipse.org/latest/index.jsp?topic=%2Forg.eclipse.platform.doc.user%2Ftasks%2Ftimpandexp.htm) to import the `epf` Eclipse preferences file. Follow the prompts to import the compiler settings.

Note that it is not *exactly* the same---the autograding feature uses the `javac` compiler and Eclipse uses its own built-in Java compiler instead.

## Javadoc Configuration

Your code should have proper Javadoc comments for *all* members and methods before requesting a code review. To receive warnings when you are missing something (or something is out of date) with your Javadoc comments, use these settings:

![Javadoc]({{ "/images/eclipse/java-compiler-javadoc.png" | relative_url }}){: .is-600 }

If you imported the compiler settings from before, this should already be configured for you!

## Code Formatting

Your code should always be consistently formatted, *especially* before requesting a code review. The exact style you adopt is up to you. The official Java code conventions are at:

  - [Code Conventions for the Java Programming Language](https://www.oracle.com/technetwork/java/codeconvtoc-136057.html), Revised April 1999.

However, these conventions are no longer maintained and major features have been introduced to Java since these were last updated. For more modern guides, see:

  - [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html), Updated May 2018.

  - [Android Open Source Project (AOSP) Java Code Style](https://source.android.com/setup/contribute/code-style), Updated January 2022.

You can also [customize the formatter](https://help.eclipse.org/latest/topic/org.eclipse.jdt.doc.user/reference/preferences/java/codestyle/ref-preferences-formatter.htm) to your own preferences. You can use these settings to change the brace style, where newlines and spaces are used, and fix how Eclipse formats try-with-resources code blocks. For example:

![Java Code Formatter]({{ "/images/eclipse/java-editor-code-style.png" | relative_url }}){: .is-600 }

You can further customize Eclipse to re-format your code every time you save. See below for details.

## Other Customization (Optional)

I recommend you customize other features in Eclipse as well. For example:

  - Change the fonts used in the editor ([reference](https://help.eclipse.org/latest/topic/org.eclipse.platform.doc.user/tasks/tasks-20.htm)). You can find several options on [Google Fonts](https://fonts.google.com/?category=Monospace). My favorites are [Fira Code](https://fonts.google.com/specimen/Fira+Code) (not Fira Mono), [Source Code Pro Light](https://fonts.google.com/specimen/Source+Code+Pro), [Roboto Mono Light](https://fonts.google.com/specimen/Roboto+Mono), [Anonymous Pro](https://fonts.google.com/specimen/Anonymous+Pro), and [Incosolata](https://fonts.google.com/specimen/Inconsolata). There are many fans of the customizable [Input](http://input.fontbureau.com/) font as well.

    ![Colors and Fonts]({{ "/images/eclipse/appearance-colors-and-fonts.png" | relative_url }}){: .is-600 }

    <i class="fas fa-info-circle"></i>
    Fonts like **Fira Code** have some fancy ligatures so the combination of characters like `<` and `=` turn into `<=` when viewed. If you copy and paste `<=` elsewhere, you'll see that it is stored as the two separate characters `<` and `=` (just displayed as if they were one).
    {: .notification }

  - Change your save actions ([reference](https://help.eclipse.org/latest/topic/org.eclipse.jdt.doc.user/reference/preferences/java/editor/ref-preferences-save-actions.htm)). I usually remove unused imports and fix indentation at least.

      ![Save Actions]({{ "/images/eclipse/java-editor-save-actions.png" | relative_url }}){: .is-600 }

  - Customize the autocomplete code templates ([reference](https://help.eclipse.org/latest/topic/org.eclipse.jdt.doc.user/reference/preferences/java/codestyle/ref-preferences-code-templates.htm), [examples](https://stackoverflow.com/questions/1028858/useful-eclipse-java-code-templates)). I usually create one for `printf` myself. You can download the templates I use and import them into your Eclipse workspace:

      <script src="https://gist.github.com/sjengle/32b18311714dc62124cb2154339288b2.js"></script>

      ![Templates]({{ "/images/eclipse/java-templates.png" | relative_url }}){: .is-600 }

  - Change the layout and add [different views](https://help.eclipse.org/latest/topic/org.eclipse.platform.doc.user/tasks/tasks-3.htm). I usually add the Tasks view, which shows me all of my `TODO` comments. I also prefer to add the Git views to my Java perspective rather than switching to the Git perspective.

      ![Views]({{ "/images/eclipse/eclipse-views.png" | relative_url }}){: .is-600 }

  - Customize the visibility of whitespaces in the text editor ([reference](https://help.eclipse.org/latest/index.jsp?topic=%2Forg.eclipse.platform.doc.user%2Freference%2Fref-texteditorprefs.htm&cp%3D0_4_1_50)). This can be helpful to detect when a mix of tabs and spaces are used for indentation.

      ![Whitespace]({{ "/images/eclipse/general-editors-text.png" | relative_url }}){: .is-600 }

      ![Whitespace]({{ "/images/eclipse/general-editors-text-whitespace.png" | relative_url }}){: .is-400 }

  - Change how content assist works for the Java editor ([reference](https://help.eclipse.org/latest/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fpreferences%2Fjava%2Feditor%2Fref-preferences-content-assist.htm)). If you find Eclipse inserting code automatically when you do *NOT* want it to, you might consider changing the "Disable insertion triggers" setting:

      ![Content Assist]({{ "/images/eclipse/java-editor-content-assist.png" | relative_url }}){: .is-600 }

The official guides for Eclipse are located at:

  - <https://help.eclipse.org/latest/index.jsp>
  {: style="list-style-type: none;" }

Keep in mind that Eclipse is under active development, and [bugs happen](https://bugs.eclipse.org/bugs/).

<i class="fas fa-info-circle"></i>
Try to <a href="https://help.eclipse.org/latest/topic/org.eclipse.platform.doc.user/tasks/tasks-47.htm">close projects in Eclipse</a> when you are no longer working on them. This can improve performance.
{: .notification }
