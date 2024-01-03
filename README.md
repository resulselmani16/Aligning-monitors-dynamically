Aligning Double Monitors Using Automator
Author: Resulselmani
Date: 31 December 2023

Introduction
This README guides you through setting up your dual monitor alignment using macOS's Automator and displayplacer. It aims to simplify the process of switching between horizontal and vertical monitor alignments with easy keyboard shortcuts, enhancing your productivity and workspace flexibility.

Tools and Libraries
displayplacer: A command-line utility for macOS to configure monitor arrangements.
Automator: An app in macOS for automating repetitive tasks.
Prerequisites
Installation of displayplacer: Use Homebrew with the following command:
brew tap jakehilborn/jakehilborn && brew install displayplacer
Knowledge in Automator and macOS Keyboard Shortcuts: Familiarize yourself with macOS's Automator app and customizing keyboard shortcuts.
Setup Instructions
Automator Quick Action:

Open Automator and select 'New Document'.
Choose 'Quick Action' as the document type.
Adding the Shell Script:

Select "Run Shell Script" from the actions list.
Paste your alignment script in the text area.
Making Script Executable:

Go to "Options" of the "Run Shell Script" action.
Ensure "Show this action when the workflow runs" is checked.
Assigning Keyboard Shortcut:

Go to System Preferences > Keyboard > Shortcuts > Services > General.
Find your Quick Action and assign a keyboard shortcut to it.
Script Execution
Once the setup is complete, use the designated keyboard shortcut to change the monitor alignment. The screen should adjust immediately to the new configuration.

Scripts
Horizontal Alignment Script
shell
Copy code
# [export PATH="/usr/local/bin:$PATH"

display_ids=($(displayplacer list | grep "Persistent screen id:" | awk '{print $4}'))

PRIMARY_DISPLAY=${display_ids[0]}
SECONDARY_DISPLAY=${display_ids[1]}

primaryWidth=$(displayplacer list | grep "$PRIMARY_DISPLAY" | sed -n 's/.*res:\([0-9]*\)x[0-9]*.*/\1/p')

displayplacer "id:$PRIMARY_DISPLAY res:1440x900 hz:60 color_depth:8 enabled:true scaling:on origin:(0,0) degree:0" "id:$SECONDARY_DISPLAY res:1920x1080 origin:($primaryWidth,-180)"]
Vertical Alignment Script
shell
Copy code
# [export PATH="/usr/local/bin:$PATH"

display_ids=($(displayplacer list | grep "Persistent screen id:" | awk '{print $4}'))

PRIMARY_DISPLAY=${display_ids[0]}
SECONDARY_DISPLAY=${display_ids[1]}

primaryHeight=$(displayplacer list | grep "$PRIMARY_DISPLAY" | sed -n 's/.*res:[0-9]*x\([0-9]*\).*/\1/p')

displayplacer "id:$PRIMARY_DISPLAY res:1440x900 hz:60 color_depth:8 enabled:true scaling:on origin:(0,0) degree:0" "id:$SECONDARY_DISPLAY res:1920x1080 origin:(-280,-$primaryHeight)"]
Customization
Feel free to adjust the scripts according to your monitor's resolution or orientation preferences. Ensure that your custom keyboard shortcuts do not conflict with existing ones.

Conclusion
By following these instructions, you should have a flexible and quick way to align your monitors according to your workflow needs. Happy productive work!
