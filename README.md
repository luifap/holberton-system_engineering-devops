What is "the Shell"?
Simply put, the shell is a program that takes commands from the keyboard and gives them to the operating system to perform. In the old days, it was the only user interface available on a Unix-like system such as Linux. Nowadays, we have graphical user interfaces (GUIs) in addition to command line interfaces (CLIs) such as the shell.

On most Linux systems a program called bash (which stands for Bourne Again SHell, an enhanced version of the original Unix shell program, sh, written by Steve Bourne) acts as the shell program. Besides bash, there are other shell programs that can be installed in a Linux system. These include: ksh, tcsh and zsh.

What's a "Terminal?"
It's a program called a terminal emulator. This is a program that opens a window and lets you interact with the shell. There are a bunch of different terminal emulators you can use. Most Linux distributions supply several, such as: gnome-terminal, konsole, xterm, rxvt, kvt, nxterm, and eterm.

Starting a Terminal
Your window manager probably has a way to launch a terminal from the menu. Look through the list of programs to see if anything looks like a terminal emulator. If you are a KDE user, the terminal program is called "konsole," in Gnome it's called "gnome-terminal." You can start up as many of these as you want and play with them. While there are a number of different terminal emulators, they all do the same thing. They give you access to a shell session. You will probably develop a preference for one, based on the different bells and whistles each one provides.

Testing the Keyboard
OK, let's try some typing. Bring up a terminal window. You should see a shell prompt that contains your user name and the name of the machine followed by a dollar sign. Something like this:

[me@linuxbox me]$

Excellent! Now type some nonsense characters and press the enter key.

[me@linuxbox me]$ kdkjflajfks

If all went well, you should have gotten an error message complaining that it cannot understand you:

[me@linuxbox me]$ kdkjflajfks

bash: kdkjflajfks: command not found

Wonderful! Now press the up-arrow key. Watch how our previous command "kdkjflajfks" returns. Yes, we have command history. Press the down-arrow and we get the blank line again.

Recall the "kdkjflajfks" command using the up-arrow key if needed. Now, try the left and right-arrow keys. You can position the text cursor anywhere in the command line. This allows you to easily correct mistakes.

You're not logged in as root, are you?
If the last character of your shell prompt is # rather than $, you are operating as the superuser. This means that you have administrative privileges. This can be potentially dangerous, since you are able to delete or overwrite any file on the system. Unless you absolutely need administrative privileges, do not operate as the superuser.

Using the Mouse
Even though the shell is a command line interface, the mouse is still handy.

Besides using the mouse to scroll the contents of the terminal window, you can copy text with the mouse. Drag your mouse over some text (for example, "kdkjflajfks" right here on the browser window) while holding down the left button. The text should highlight. Release the left button and move your mouse pointer to the terminal window and press the middle mouse button (alternately, you can press both the left and right buttons at the same time if you are working on a touch pad). The text you highlighted in the browser window should be copied into the command line.

A few words about focus...
When you installed your Linux system and its window manager (most likely Gnome or KDE), it was configured to behave in some ways like that legacy operating system.

In particular, it probably has its focus policy set to "click to focus." This means that in order for a window to gain focus (become active) you have to click in the window. This is contrary to traditional X Window behavior. You should consider setting the focus policy to "focus follows mouse". You may find it strange at first that windows don't raise to the front when they get focus (you have to click on the window to do that), but you will enjoy being able to work on more than one window at once without having the active window obscuring the the other. Try it and give it a fair trial; I think you will like it. You can find this setting in the configuration tools for your window manager.

Previous | Contents | Top | Next

© 2000-2020, William E. Shotts, Jr. Verbatim copying and distribution of this entire article is permitted in any medium, provided this copyright notice is preserved.

Linux® is a registered trademark of Linus Torvalds.