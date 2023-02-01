---
title: Project Grading
navbar: Resources
layout: resources
key: 0.3

tags:
 - text: 'Pending'
#    type: 'is-info'

subsections:
  - text: 'Project Tests'
    link: '#project-tests'

  - text: 'Project Reviews'
    link: '#project-reviews'

  - text: 'Project Design'
    link: '#project-design'

  - text: 'Project Timeline'
    link: '#project-timeline'

---

Project grades are separated into multiple parts: **tests**, **reviews**, and **design**. The **tests** grade indicates your project passes the functionality requirements, the **review** grades track your code reviews with the instructor, and the **design** grade indicates your project passes the design requirements.

Projects use **mastery learning** for grading. You must complete *all* of the requirements to earn a grade. If you complete those requirements on-time, you will receive a 100%. Otherwise, the standard late policy applies.

All grade and code review requests start the same way: creating an issue using the correct template. This will trigger the GitHub Actions bot to run the necessary checks in the background. Pay attention to your GitHub notifications---the bot will respond to your issue within a few minutes with further instructions!

Complete the [Project Setup](setup.html) and [Project Testing](testing.html) guides before this one.

## Project Tests
{: .page-header }

You may request the "Project Tests" grade of a project once you have a release with the correct version number that passes the test checks (see below). If your release meets these eligibility criteria, use the "Request Project Tests Grade" issue template to request this grade.

### Release Version

You must have a release with the correct version number in the form `v[X].[Y].[Z]` where the **major** number `[X]` is the project number, the **minor** number `[Y]` starts at `0` and reflects the number of past code reviews for this project, and the **patch** number `[Z]` starts at `0` and is incremented as necessary.

For example, the first release you create for project 1 should be numbered `v1.0.0`. If it does not pass the tests, then you can make fixes to the code and a new `v1.0.1` release. The patch number keeps incrementing until you have a release that passes the test checks.

### Test Checks

The following test checks must pass to be eligible for this grade:

  1. Your release must pass the functionality tests found in the `test-v1.x` group of JUnit tests when run on GitHub actions, where `x` is the minor version number.

  1. Your release must NOT pass the functionality tests for the future projects. The functionality for future projects must be in a separate branch until the design of the current project is approved through code review.

  1. For projects 2, 3, and 4, you must have a non-zero grade for the "Project vX.0 Review" assignment in Canvas, where `X` is the previous project number.

<i class="fas fa-info-circle"></i>
Even though your future releases of this project still need to pass the test checks for code review, you only need to request a test grade once. 
{: .notification }

## Project Reviews
{: .page-header }

You may request code reviews for a project once you have a non-zero grade for the "Project Tests" assignment in Canvas and have a release with the correct version number that passes the review checks (see below). 

If your release meets these eligibility criteria, use the "Request Project Code Review" issue template to request a code review appointment. To receive credit for your first two code reviews, use the "Request Project Review Grade" issue template to request those grades *after* the appointment has concluded.

### Release Version

You must have a release with the correct version number in the form `v[X].[Y].[Z]` where the major number `[X]` is the project number, the minor number `[Y]` is the number of *completed* code reviews, and the patch number `[Z]` starts at `0` and is incremented as necessary.

For example, the first release you create for project 1 should be numbered `v1.0.0` since you do not yet have any completed code reviews. After you attend code review and merge the changes, the next release should be numbered `v1.1.0`. If you need to make another change before your next code review appointment appointment, the patch number increments to `v1.1.1` and so on.

### Review Checks

The following review checks must pass to be eligible for code review:

  - Your release must be in the `main` branch of your project repository. The functionality for future projects must be in a separate branch until the design of the current project is approved.

  - Your release must still meet the [test checks](#test-checks) for this project.

  - Your release must not have any `TODO` comments in the code.

  - Your release must not have any compile or Javadoc warnings in the code.

  - Your release must use proper code style and naming conventions.

  - Your release must not output stack traces to the console.

  - Your release must address the `TODO` comments from the previous code review (if applicable).

While GitHub Actions attempts to check for these requirements, these checks are not perfect. You must do your best to manually check your code for these requirements.

The first code review for each project will be 30 minutes. The first code review has the following additional requirements:

  - You must have a non-zero grade for the "Project `[X]` Tests" assignment of the current project, where `[X]` is the current project number.

  - For projects 2, 3, and 4, you must have a non-zero grade for the "Project `[X-1]` Design" assignment, where `[X-1]` is the previous project number. (This prevents you from working too far ahead while the codebase is unstable.)

Only the first two code review appointments are graded. However, you must continue the code review process until the instructor approves your design and you receive a **PASS** or **CONDITIONAL PASS** in your code review.

### Review Limits

**You may only have up to 30 minutes maximum of code reviews every ~~5~~ 4 days.** That works out to one 30 minute appointment or two 15 minute appointments per ~~5~~ 4 day period. 

Most projects take 3 to 5 code reviews to complete the design. Due to weekends, that works out to approximately 2 to 4 weeks to complete the code review process. You may have as many code reviews as necessary to pass the design, but too many reviews will prevent progress on the next project and may result in late penalties down the line.

### Review Signup

Use the "Request Project Code Review" issue template to request a code review appointment. It is important you **do not misuse the appointment system** when signing up. This includes:

  - Do not sign up for an appointment time before you are eligible. You may have up to 30 minutes of code reviews every 5 days. The instructions will include the *earliest* date you can signup.

  - Do not sign up for more than one appointment at a time. You may only have 1 upcoming code review appointment at a time.

  - Do not miss your appointment or arrive more than 5 minutes late.

<i class="fas fa-exclamation-triangle"></i>
You will receive one warning if you misuse the code review appointment system. After a warning, you may choose one of two penalties per offense: (0) lose **5** points to your project grade, or (0) wait another **5** days for your next appointment.
{: .notification .is-danger .is-light }

## Project Design
{: .page-header }

You may request the "Project Design" grade of your project after you receive a **PASS** or **CONDITIONAL PASS** from the instructor in code review and have a release with the correct version number that passes the design checks (see below). 

If your release meets these eligibility criteria, use the "Request Project Design Grade" issue template to request this grade. Once the grade is updated on Canvas, you can merge the functionality of the next project into the `main` branch on GitHub and begin the code review process for that project.

### Release Version

You must have a release with the correct version number in the form `v[X].[Y+1].[Z]` where the major number `[X]` is the project number, the minor number `[Y+1]` is the code review number that passed plus 1, and the patch number `[Z]` starts at `0` and is incremented as necessary.

For example, suppose release `v1.4.2` passed code review for project 1. The next release you create should be numbered `v1.5.0`. If you need to make changes after that, the next release should be numbered `v1.5.1`. The patch number keeps incrementing until you are ready to request the design grade.

### Design Checks

The following design checks must pass to be eligible for this grade:

  - Your release must still meet the [test checks](#test-checks) for this project.

  - Your release must still meet the [review checks](#review-checks) for this project.

  - You must have a code review pull request on GitHub approved with the **PASS** or **CONDITIONAL PASS** status by the instructor.

  - You must have a non-zero grade for the "Project `[X]` Tests" assignment of the current project, where `[X]` is the current project number.

  - You must have a non-zero grade for the "Project `[X]` Review 1" assignment of the current project, where `[X]` is the current project number.

  - You must have a non-zero grade for the "Project `[X]` Review 2" assignment of the current project, where `[X]` is the current project number.

<i class="fas fa-info-circle"></i>
There is no "Project 4 Design" grade due to lack of time at the end of the semester. Instead, there is only a "Project 4 Review" grade for the final code review where the search engine functionality is evaluated.
{: .notification }

## Project Timeline
{: .page-header }

Time management is critical to doing well in this course. The following table provides a timeline that will make sure you make it to the final search engine project. In most weeks, you will be working on two projects at a time: the tests of the next project, and the design (or code reviews) for the current project.

<style>
tbody > tr:nth-child(n+1):nth-child(-n+4) > td:nth-child(2),
tbody > tr:nth-child(n+4):nth-child(-n+7) > td:nth-child(3) {
  background-color: #fffaeb;
}

tbody > tr:nth-child(n+5):nth-child(-n+9) > td:nth-child(2),
tbody > tr:nth-child(n+8):nth-child(-n+12) > td:nth-child(3) {
  background-color: #effaf5;
}

tbody > tr:nth-child(n+9):nth-child(-n+12) > td:nth-child(2),
tbody > tr:nth-child(n+12):nth-child(-n+15) > td:nth-child(3) {
  background-color: #eff5fb;
}

tbody > tr:nth-child(n+13):nth-child(-n+15) > td:nth-child(2),
tbody > tr:nth-child(n+16) > td:nth-child(3) {
  background-color: hsl(0, 0%, 96%);
}
</style>

| Week   | Project Tests | Project Design | Notes             |
|:------:|:-------------:|:--------------:|:------------------|
| 2      | Project 1     | N/A            |                   |
| 3      | Project 1     | N/A            |                   |
| 4      | Project 1     | N/A            |                   |
| 5      | Project 1     | Project 1      |                   |
| 6      | Project 2     | Project 1      | Conference Travel |
| 7      | Project 2     | Project 1      |                   |
| 8      | Project 2     | Project 1      | Exam 1            |
| 9      | Project 2     | Project 2      | Exam 1 Retake Due |
| 10     | Project 3     | Project 2      |                   |
| 11     | Project 3     | Project 2      | Project Cutoff    |
| 12     | Project 3     | Project 2      |                   |
| 13     | Project 3     | Project 3      |                   |
| 14     | Project 4     | Project 3      | Thanksgiving      |
| 15     | Project 4     | Project 3      | Exam 2            |
| 16     | Project 4     | Project 3      | Exam 2 Retake Due |
| Finals | N/A           | Project 4      | Finals Week       |
{: .is-auto .table .is-hoverable }

Each week you fall behind this schedule reduces your ability to make it to the final project. If you fall too far behind, you simply do not have enough weeks left to catch up and pass the class. You need approximately 3 weeks for just the code review process alone!

Keep in mind that a fixed number of code reviews are available every week. **If you do not take advantage of appointments early in the semester, those code review opportunities are lost**; you cannot expect multiple appointments per week at the end of the semester hoping to catch up.

