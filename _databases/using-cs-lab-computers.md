---
title: Using CS vLab Computers
navbar: Resources
layout: resources
key: 0.1

tags:
  - text: 'New'
    type: 'is-primary'
    
# MUST USE VLAB COMPUTERS!

---

You must use your CS account and CS **virtual** lab (vlab) computers to connect to our MariaDB database.

## Account Basics

Your CS account shares the same username as your myUSF account, but **different** passwords. You should use your CS account for the following purposes:

  - Logging into the computers in the CS classrooms (Harney 142 and LS G12).
  - Logging into the computers in the CS labs (Harney 413).
  - **Logging into the virtual lab (vlab) computers remotely via SSH.**

Alternatively, you use your myUSF username and password to login into myUSF, Canvas, access your `@dons.usfca.edu` email, and other university-wide resources.

## Username and Password

Your CS account username will always be the same as your myUSF username. You can determine your username by looking at the part before `@dons.usfca.edu` in your USF email address. For example, if my email address is `sjengle@dons.usfca.edu` then my username will be `sjengle`.

Your initial password is your student ID. For example, if your student ID is `20123456`, then your initial password will be `20123456`. You should [change your password](https://myusf.usfca.edu/arts-sciences/computer-science/technical-resources#q3) after logging in for the first time.

## Remote SSH Access

If you want to access one of the CS vlab computers remotely (for example, from home or off-campus), you must [login using SSH](https://myusf.usfca.edu/arts-sciences/computer-science/technical-resources) to our [gateway](https://en.wikipedia.org/wiki/Gateway_(telecommunications) system. From a Linux, Mac, or updated Windows 10 system, open a Terminal window and enter the following command with your own CS username:

<input type="text" class="input is-expanded is-family-code" value="ssh USERNAME@stargate.cs.usfca.edu"/>

**Hint:** You can edit the text above to replace `USERNAME` with your CS username before copying/pasting the command.
{: .notification .is-success }

This will log you into `stargate.cs.usfca.edu`, which is our network gateway that provides access to the rest of our CS network. However, Java and `mysql` are **not** installed on our gateway! You must next log into one of our **virtual** lab (vlab) computers. 

Enter `rusers` to find which systems are free. Look for a system that starts with `vlab` and has no (or very few) users listed next to its name. Then, SSH into that computer. For example, if `vlab20.cs.usfca.edu` is free, you can enter:

<input type="text" class="input is-expanded is-family-code" value="ssh vlab20"/>

You do not need to include your username or the domain "cs.usfca.edu" above, since you are already logged into the `stargate` gateway on the CS network.

## Getting Help

Your CS account is created and managed by our [CS Support](https://www.cs.usfca.edu/support.html) team. If you run into any issues with your account, please contact them via email at <support@cs.usfca.edu>.

The instructor **cannot** reset your password for you, but we do have a test account you can use temporarily if you are having trouble.
