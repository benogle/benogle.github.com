---
layout: post
title: "Windows/linux developers: remap your mac"
status: publish
type: post
published: true
keywords: remap mac, mac keyboard, mac, linux to mac, apple, windows to mac
description: A post detailing how to remap your mac keyboard to cater to your windows or linux bred habits in using the control key. Away with the command key!
archived: true
---

Up until recently, I have been writing code on windows and linux machines. All that has changed.
The folks at the newish job got me a shiny new macbook. And while it is by far the best machine
I've ever owned, it is not without flaws: the command and option keys. W-T-F?

When I use the computer, the majority of my time is spent moving around in some text editor with
the keyboard. Here is my gripe: the control, option, and command keys do the same work as the
control key on other systems. I tried this configuration for a couple weeks and almost went
crazy. Command+right moves you to the end of the line, WTF?! The end key (when you find it)
moves you to the end of the file, WTF?! Option+right moves you to the next word, WTF?!
Ctrl+arrows just make a bonk noise, WTF?! And on top of that, I couldn't use my pinky to handle
cut/copy/paste actions. GRRRRR.

So how do we fix this mac brokeassness? My approach was to essentially swap the command and
control keys, then fix bindings here and there until I no longer got frustrated. It takes a bit
of work. So lets get going.

First: Swap the command and control keys
-----------------------------------------

 * Click: apple icon -> System Preferences... -> keyboard and mouse -> Modifier Keys...
 * Then make the command key the control key, and vice versa.

This is the easiest part, and it gets you some pretty solid progress. Proper ctrl+C, ctrl+V!
Yay! But this alone still leaves a lot to be desired.

Second: Remap text editor actions
---------------------------------

Here you can remap the actions that move your cursor around in any native text widget such as
TextEdit or textareas in Safari (note that it does not affect FireFox or any other browser). If
you are a programmer, this will likely be important to you.

Basically, you create a file: `~/Library/KeyBindings/DefaultKeyBinding.dict` and put key binding
information in it. An explanation is at
[heisencoder](http://heisencoder.net/2008/04/fixing-up-mac-key-bindings-for-windows.html), and a
list of all possible options is at [Erase to the left](http://www.erasetotheleft.com/post/mac-os-x-key-bindings).

My file looks like this:

{% highlight javascript %}
/* ~/Library/KeyBindings/DefaultKeyBinding.Dict
This file remaps the key bindings of a single user on Mac OS X 10.5 to more closely
match default behavior on Windows systems.  This particular mapping assumes
that you have also switched the Control and Command keys already.

This key mapping is more appropriate after switching Ctrl for Command in this menu:
Apple->System Preferences->Keyboard & Mouse->Keyboard->Modifier Keys...->
Change Control Key to Command
Change Command key to Control
This applies to OS X 10.5 and possibly other versions.

Here is a rough cheatsheet for syntax.
Key Modifiers
^ : Ctrl
$ : Shift
~ : Option (Alt)
@ : Command (Apple)
# : Numeric Keypad

Non-Printable Key Codes

Up Arrow:     \UF700        Backspace:    \U0008        F1:           \UF704
Down Arrow:   \UF701        Tab:          \U0009        F2:           \UF705
Left Arrow:   \UF702        Escape:       \U001B        F3:           \UF706
Right Arrow:  \UF703        Enter:        \U000A        ...
Insert:       \UF727        Page Up:      \UF72C
Delete:       \UF728        Page Down:    \UF72D
Home:         \UF729        Print Screen: \UF72E
End:          \UF72B        Scroll Lock:  \UF72F
Break:        \UF732        Pause:        \UF730
SysReq:       \UF731        Menu:         \UF735
Help:         \UF746

NOTE: typically the Windows 'Insert' key is mapped to what Macs call 'Help'.
Regular Mac keyboards don't even have the Insert key, but provide 'Fn' instead,
which is completely different.
*/

{
"\UF729"   = "moveToBeginningOfLine:";                       /* Home         */
"@\UF729"  = "moveToBeginningOfDocument:";                   /* Cmd  + Home  */
"$\UF729"  = "moveToBeginningOfLineAndModifySelection:";     /* Shift + Home */
"$@\UF729" = "moveToBeginningOfDocumentAndModifySelection:"; /* Shift + Cmd  + Home */
"\UF72B"   = "moveToEndOfLine:";                             /* End          */
"@\UF72B"  = "moveToEndOfDocument:";                         /* Cmd  + End   */
"$\UF72B"  = "moveToEndOfLineAndModifySelection:";           /* Shift + End  */
"$@\UF72B" = "moveToEndOfDocumentAndModifySelection:";       /* Shift + Cmd  + End */

"^\UF702"   = "moveToBeginningOfLine:";                       /* Home         */
"^\UF703"   = "moveToEndOfLine:";                             /* End          */
"$^\UF703"  = "moveToEndOfLineAndModifySelection:";           /* Shift + End  */
"$^\UF702"  = "moveToBeginningOfLineAndModifySelection:";     /* Shift + Home */

"\UF72C"   = "pageUp:";                                      /* PageUp       */
"\UF72D"   = "pageDown:";                                    /* PageDown     */
"@x"  = "cut:";                                         /* Shift + Del  */
"@v"  = "paste:";                                       /* Shift + Help */
"@c"  = "copy:";                                        /* Cmd  + Help (Ins) */
"@\UF702"  = "moveWordBackward:";                            /* Cmd  + LeftArrow */
"@\UF703"  = "moveWordForward:";                             /* Cmd  + RightArrow */
"$@\UF702" = "moveWordBackwardAndModifySelection:";   /* Shift + Cmd  + Leftarrow */
"$@\UF703" = "moveWordForwardAndModifySelection:";   /* Shift + Cmd  + Rightarrow */
}
{% endhighlight %}

Third: Fixing/Rebinding Command Tab
-----------------------------------

Swapping your command and control keys breaks the standard command+tab application switcher. Now
it is ctrl+tab, which is handy for switching between tabs in an application.

OSX does not allow you to remap the standard Command+Tab (or in our case Ctrl+Tab) behavior. To
get around this, you need to use a program called
[PullTab](http://www.ragingmenace.com/software/pulltab/index.html). PullTab disables the native
application switcher. Then you can install a new, better application switcher called
[witch](http://www.manytricks.com/witch/). There is a
[screencast available](http://www.screencastcentral.com/public/yt4.cfm) to help you along.

This will allow you to bind command+tab to other actions such as tab switching in your browser.
I mapped control(the command key)+tab to the application switcher so the action feels like
alt+tab.

Fourth: Fixing the home and end keys in the terminal
-----------------------------------------------------

It seems the mac's default behavior for the home and end keys is to put the cursor all the way
at the beginning and end of the document. This is even true in the terminal. Home will scroll
you all the way to the beginning of the terminal's buffer! Whoa. I expect the home and end keys
to put you at the beginning or end of the line. This can be changed.

 * Open terminal and open the preferences from the terminal menu
 * Click the 'keyboard' tab to show all the key bindings
 * Find 'end' which should say 'scroll to end of buffer' and change it to `\033[F`
 * Find 'home' which should say 'scroll to start of buffer' and change it to `\033[H`

Fifth: Deal with miscellany
---------------------------

There are a few applications I could not figure out how to remap. Firefox is my main point of
contention. HTML text boxes do not respond to the text editor key binding changes listed in
number 2 of this post, and I have not been able to figure out how to remap them in the
application itself. It isn't a big deal, but would be nice.

DONE!
-----

Well, I hope this helps. You should have a mac that now responds to your old habits! On with it!
