# NewNoteFromSelection-VSCode
Creates a new markdown file with selected text and replaces original text with link to new file

![](Selection2Note.gif)

1. Need to install [Macros](https://marketplace.visualstudio.com/items?itemName=geddski.macros) extension
1. Open your User settings by pressing Ctrl+Shift+P and tpying "Open Settings (JSON)" or locating settings.json at the following paths
    - Windows %APPDATA%\Code\User\settings.json
    - MacOS $HOME/Library/Application Support/Code/User/settings.json
    - Linux $HOME/.config/Code/User/settings.json
1. Add the following code to the bottom of the file: 
```
"macros.list": {
        "updateNote": [
            {
                "command": "$delay",
                "args": {
                    "delay": 500
                }
            },
            "workbench.action.files.save",
            "editor.action.insertLineAfter",
            "editor.action.insertLineAfter",
            "editor.action.clipboardPasteAction",
            "cursorTop",
            "fileutils.copyFileName",
            "editor.action.clipboardPasteAction",
            "deleteLeft",
            "deleteLeft",
            "deleteLeft", 
            "cursorHomeSelect",
            "editor.action.clipboardCopyAction",
            "cursorTop",
            {"command": "type", "args": {"text": "# " }},
            "workbench.action.navigateBack",
            "deleteRight",
            {"command": "type", "args": {"text": "[[" }},
            "editor.action.clipboardPasteAction",             
        ],
       
        "copySelection": [
            "editor.action.clipboardCopyAction"
        ],         
    },
```
1. Open the user tasks file by entering Ctrl+Shift+P -> "Tasks: Open User Tasks". This file can also be found (or created) at the same user settings folder paths mentioned in step 2. 
1. Insert the following code into the file. If you don't have any existing tasks, you can replace the entire code. 
```
{
    "version": "2.0.0",
    "tasks": [
      {
        "label": "newNote",
        "type": "shell",
        "command": "echo '' > ${input:fileName}.md | code '${input:fileName}.md'", 
      },
      {
        "label": "copyText",
        "type": "shell",
        "command": "${command:macros.copySelection}"     
      },
      {
        "label": "newNoteFromSelection",
        "type": "shell",
        "command": "${command:macros.updateNote}",
        "dependsOrder": "sequence",
        "dependsOn": ["copyText", "newNote"]
      }
    ],
  
    "inputs": [
      /* {
        "type": "promptString",     
        "id": "folderName",
        "description": "Complete my folder name.",
        "default": "folder"
      }, */
      {
        "type": "promptString",
        "id": "fileName",
        "description": "Complete my file name.",
        "default": "new file name",
      }
    ]
  }
```

1. Open the user Keybindings file, by either entering Ctrl+Shift+P -> "Preferences: Open Keyboard Shortcuts (JSON) or opening this file from the same directory listed in Step 2.
1. At the bottom, add the following code and customize the keyboard shortcut to your preference.
```
{
  "key": "ctrl+alt+shift+n",    // whatever you choose
  "command": "workbench.action.tasks.runTask",
  "args": "newNoteFromSelection"
},
      
```

1. Open the markdown file with the text that you would like to extract. 
1. Select text.
1. Use the keyboard shortcut that you just set. 
1. Enter a title in the Input Box. If it has multiple words in it, consider adding "-" between each word
