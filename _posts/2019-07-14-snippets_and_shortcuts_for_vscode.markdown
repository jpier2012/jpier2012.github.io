---
layout: post
title:      "Snippets and Shortcuts for VSCode"
date:       2019-07-14 05:17:09 -0400
permalink:  snippets_and_shortcuts_for_vscode
---

As I read more and more about the day to day as a Software Engineer, I'm hearing a common theme:

Productivity matters.

Ultimately, my impact as a developer comes down to not only my technical skills, but the speed with which I can write code in a production environment. When the deadlines are coming in hot, I need to make sure I can deliver quickly and concisely. 

So, how best to ensure you're able to work as quickly as possible without burning out? Or worse, making stupid mistakes while rushing to meet a delivery?

Recruit your IDE to help! 

Most IDEs have some functionality that allows you to assign bits of code to different key combinations to speed up small, repetitive tasks that you use within the IDE. Since it took me a bit to get the hang of the whole process, I figured I shoud share my setup experience with the world.

I am writing this with reference to the Windows and Linux versions of VSCode, so the specific steps listed here may not directly apply to other IDEs. However, the concepts should still apply even if the path to creating hotkeys differs a little bit.

## First, what's the difference between the two?

The differences are subtle, but important.

Snippets are language-specific bits of code that can be tied to key words that, when typed, provide an autocomplete suggestion that can be accessed through up and down arrows and then selected with "tab".

![](https://i.imgur.com/t1FImmC.png)
* *After typing "form" I am prompted to autocomplete using my snippets.*


In this case, I've created two separate "form" snippets that each create a basic frame for either form_for or form_tag erb scripts. This generates both the form as a whole and the submit button, then places the cursor at the point where you first declare the path to which the form submits, as well as a declared variable if there is one.

![](https://i.imgur.com/9UDJyhi.png)
* *form_for and form_tag snippet outputs respectively*


Shortcuts, on the other hand, are reserved for processes that occur within the IDE. An easy example of this is saving the document by using ctrl+s, or hiding the sidebar menu with ctrl+b. 

You *can* use shortcuts as snippets, however, it's a bit clunkier to use these and you're not able to position the cursor once the shortcut has been called, though it's also slightly faster to use a keybinding than a snippet because you don't have to make any selections from the autocomplete menu. For small bits of code like "<br>", "end", or even "<% end %>" tags, I think you can use shortcuts just as easily as snippets.

## So how do I get to them?

It's easy!

To access snippets, go to File > Preferences > User Snippets. From there you'll be prompted with a menu allowing you to choose the language for which you're writing the snippet.

Once you've got the snippet edit file open - in this case it's ruby.json - you'll see the all-too-familiar json attribute hash syntax. Here's the code for the "form_for" snippet shown above:

```
# 1) "form_for": {
# 2)	"prefix": "form",
# 3)	"description": "Create ERB form_for",
# 4)	"body": [ "<%= form_for $0 do |f| %>\n  <%= f.submit %><br>\n<% end %>" ]
}
```

Let's break it down -

![""](https://imgur.com/irvN9Of.png)

1) "form_for" is the name of the snippet, which displays next to the prefix shown in the autocomplete prompt.

2) the "prefix" is whatever character, or series of characters you want to use to prompt you for the snippet. In this case, when I'm only prompted for the form snippets after typing "form", however, this prefix can be set to any sequence of characters.

3) a brief description.

4) the "body" is the output generated when the snippet is called. 

Notice the "$0" global variable in the code block above the screenshot - this represents where your active cursor will be in the output. You can add multiple active cursors using variables $1, $2, etc. 

"\n" is an escape character used to generate a new line within the body text to keep the script properly organized.  Without those in there, the ouput would be exactly "<%= form_for $0 do |f| %>  <%= f.submit %><br><% end %>" , which isn't good for anyone.

Here are a few more examples to pique your inspiration (though I'm still working on actively using them):

```
	 "h1 tag": {
	 	"prefix": "h",
	 	"body": [
			 "<h1>$0</h1>"
	 	],
	 	"description": "h1 header"
	 }

	 "h2 tag": {
	 	"prefix": "h",
	 	"body": [
			 "<h2>$0</h2>"
	 	],
	 	"description": "h2 header"
	 }

	 "h3 tag": {
	 	"prefix": "h",
	 	"body": [
			 "<h3>$0</h3>"
	 	],
	 	"description": "h2 header"
	 }

	 "method": {
	 	"prefix": "def",
	 	"body": [
			 "def $0\nend"
	 	],
	 	"description": "empty method"
	 }

	 "erb display tag": {
	 	"prefix": "e",
	 	"body": [
			 "<%= $0 %>"
	 	],
	 	"description": "ERB display tag"
	 }

	 "erb tag": {
		 "prefix": "e",
	 	"body": [
			 "<% $0 %>"
	 	],
	 	"description": "ERB tag"
	 }

	 "end tag": {
	 	"prefix": "e",
	 	"body": [
			 "<% end %>"
	 	],
	 	"description": "end ERB tag"
	 }
```

In short, there's a lot you can do with these - as an added bonus you can create language-independent snippets by selecting "New Global Snippets File" from the snippet select menu where ruby.json was initially chosen. You can also reference constants defined by Microsoft, including the currently selected body of text or the directory of the current document. You can even go as far as to alter the logic used to define which suggestions are provided in differing contexts (through the "scope" declaration), though I haven't gone that deep down the rabbit hole myself. 

[Here is the official snippet documentation from Microsoft](https://code.visualstudio.com/docs/editor/userdefinedsnippets)

-

To access keyboard shortcuts, press ctrl+k+s, so hold control, press k, and then while still holding control, press s (or use the File menu). From there you can either select a shortcut from the search menu and define by double clicking the line item, or you can edit the shortcut code directly (the fun way).

Once you're prompted with the menu, there should be some fine print providing a link to the keybindings.json file:

![""](https://imgur.com/6u6DQhP.png)

Once you click the link, two separate files will pop up, the list of default keybindings, which cannot be edited, and keybindings.json, the repository for your custom shortcuts.

These are defined in a similar fashion to snippets in that they're in JSON format:

```
  { # 1) "key": "ctrl+w", 
    # 2) "command": "type",
    # 3) "args": { "text": "<br>" },
    # 4) "when": "editorTextFocus" }
```

1) the "key" is the key combination that is used to call the shortcut.
2) "command" is the action carried out by the shortcut, in this case it's generating text.
3) "args" represents the output, with "text" defining the specific string of characters generated.
4) "when" describes the context. In this case, the shortcut is only called when the text editor window is actively selected. If I were typing away in the terminal, I would not be able to use the shortcut unless edited to include the terminal context.

One thing to keep in mind is that you can overwrite existing keybindings with this file as well - by default, ctrl+w closes the active editor window. This does not jibe with my own butterfingers, whom tend to have a mind of their own, so I purposefully overwrote ctrl+w just to output "<br>" to ensure that I don't accidentally close what I'm working on. Ctrl+q also quits VSCode immediately, but I didn't have a shortcut to add, so I just left the "args" and "when" blank to ensure the functionality is removed:

```
{ "key": "ctrl+w", "command": "",
        "args": { "text": "" },
        "when": "" }
```

This is primarily to keep it on the ready until I'm ready to assign a shortcut to ctrl+w, since right now I'm only interested in removing its default functionality.

You can also remove specific functionality for a key combination that has multiple contexts via this syntax:
{ "key": "ctrl+w", "command": "-workbench.action.closeActiveEditor" }

[Here is the official shortcut documentation from Microsoft](https://code.visualstudio.com/docs/getstarted/keybindings), which also contains links to keybinding cheat sheets for each OS.

Here are some shortcuts that I personally find extremely useful:

* **Open recent workspaces:** *ctrl+r* (while in editor)

* **Copy/paste to and from terminal:** *ctrl+shift+c* / *ctrl+shift+v*

* **Toggle sidebar:** *ctrl+b*

* **Open sidebar file list:** *ctrl+shift+e*

* **Toggle Terminal:** *ctrl+j* OR *ctrl+\`*

* **Split Terminal:** *ctrl+\\* 
 - especially useful for running a rails server or console so you can avoid flipping back and forth between terminals when debugging.
* **Editor file groupings:** *ctrl+1, ctrl+2, ctrl+3* 
 - when a file is selected in the sidebar, it will open the file within the group designated. So, if you want to open 3 files, split between 3 groups in the IDE, select each file and hit ctrl+1, ctrl+2, ctrl+3 over each respectively. Within an editor it allows you to jump to the selected group.
* **Cycle between open editors in a group:** *ctrl+tab*
 - opens editor quick select within a group. Similar functionality to using alt+tab to switch between programs in Windows.
* **Find and select next match:** *ctrl+d* 
 - extremely useful - if your cursor is resting on a word, pressing ctrl+d once will select the entire word. However, while still holding control, you can hit d again and it will incrementally find the next matching phrase and select it along with the existing selections. It's very handy for updating variable or method names without taking the leap of faith that is find and replace.
* **Create multiple cursors:** *ctrl+shift+up/down arrows* 
 - while holding control and shift you can place cursors on multiple lines to make simultaneous updates. This seems incredibly useful but I have yet to find an efficient way of using it.
* **Move line up or down:** *alt+up/down arrows*
 - when in an editor window, you can hold alt while on the current line and then use up or down to move the entire line before or after the adjacent one.
* **Scroll editor:** *ctrl+up/down arrows*

NOTE: autocomplete may not prompt depending on the positioning of your cursor within the editor, in which case you can hit *ctrl+space* to manually bring up the menu. I'm not sure why, but some logic within the VSCode editor formatting prevents autocomplete from populating depending on what other code is present in the doc. I find issues specifically when using brackets < and >.

That's all I have for today, I hope you all found this useful - thanks for reading, and happy coding!
