Aligning double monitors using Automator
========================================

![Resulselmani](https://miro.medium.com/v2/resize:fill:88:88/1*Kf7zXnZqy0NVpWQ4CdYtfg@2x.jpeg)

[Resulselmani]
6 min read

Dec 31, 2023


![](https://miro.medium.com/v2/resize:fit:1400/1*nPtpuwFEEfMe6ZYL2WSgfw.jpeg)

As someone who thrives on versatility and dynamic work environments, I often find myself adjusting my workspace to suit my changing needs throughout the day. Whether it’s for comfort, convenience, or a shift in tasks, the ability to switch between using an external keyboard for an expansive, multi-monitor setup and reverting to the compact, all-in-one laptop configuration is crucial for my productivity and ergonomics. This constant adjusting, however, used to involve tedious manual settings changes every time I wanted to realign my monitors — that is until I devised a solution. I created a script to seamlessly transition my monitor setup between horizontal and vertical alignments with simple keyboard shortcuts, catering to the fluid nature of my work and lifestyle. This blog post delves into the why and how of this project, offering insights and instructions so you can implement a similar setup for your dynamic workflow needs.

**Tools and Libraries:**

*   **displayplacer:** This is a key component of the script. displayplacer is a command-line utility for macOS that allows users to configure the arrangement of their monitors. It enables precise control over monitor positions, resolutions, and orientations, which is essential for the script’s functionality to switch between horizontal and vertical alignments.
*   **Automator**: The Automator app in macOS is a tool designed for automating repetitive tasks without needing complex programming skills. It allows users to create custom workflows to perform various functions such as renaming files, resizing images, or batching documents. Users can select from a variety of actions and arrange them into a sequence to create automated processes, which can be saved and executed as needed. Automator supports various script types and integrates with numerous macOS applications, streamlining complex tasks into simple, one-click operations.

By using macOS’s native Automator app and the displayplacer library, the script ensures compatibility and efficiency within the macOS environment. This section communicates the technical framework and tools that empower the script’s capabilities.

**Programming Approach:**

*   The script was developed as a shell script intended to run within the Automator app on macOS. This integration allows the script to be triggered through user-defined keyboard shortcuts, offering a seamless experience for changing monitor alignments.

Setup and Requirements:
=======================

**Installation of displayplacer:**

*   Users need to install the `**displayplacer**` library on their macOS. This can be done through the Terminal, typically using a package manager or directly from the source. It's a crucial step as this library is what allows the script to manipulate monitor alignments.
*   Using the terminal to install `displayplacer` goes like this:

`brew tap jakehilborn/jakehilborn && brew install displayplacer`

**Knowledge Prerequisites:**
============================

*   **Understanding Automator:** Users should have a basic understanding of how to use Automator, a macOS application that automates repetitive tasks. They need to know how to integrate the shell script into Automator to set it up for running.
*   **Customizing Keyboard Shortcuts:** Users should also be familiar with customizing keyboard shortcuts in macOS. This knowledge is necessary to assign specific shortcuts to trigger the monitor alignment changes.

**Operating System Compatibility:**

*   The script and setup are specifically designed for macOS, considering it utilizes macOS’s Automator app and displayplacer, which are tailored for this operating system.

By ensuring the proper installation of `**displayplacer**` and having a foundational understanding of Automator and macOS keyboard shortcuts, users will be well-equipped to use this script. This section ensures that anyone looking to implement the script understands the prerequisites and setup steps involved.

**Specific Steps for Automator Setup:**

*   Detailed steps on how to create a new Quick Action in Automator and where to place the shell script.
*   Instructions on making the script executable within Automator.

**Script Execution:**

*   How the script is activated once everything is set up (e.g., triggering the script using the predefined keyboard shortcut).
*   Any visual or audible confirmation that the script has run or that the monitor alignment has changed.

**Customization and Modification:**

*   Information on how users can customize or modify the script, if at all. This might include changing the keyboard shortcuts, adjusting the script for different monitor setups, or other personalizations.

**Setting Up the Quick Action in Automator:**
=============================================

**Create a Quick Action:**

*   Open the Automator app on your macOS.
*   Click on ‘New Document’ and choose ‘Quick Action’ as the type of document to create.

**Adding the Shell Script:**

*   Within Automator, navigate to the left panel and select the “Run Shell Script” option.
*   This opens a text area resembling a terminal where you can input your script. Paste or write your script here.

**Making the Script Executable:**

*   Once your script is ready, go to the “Options” of the “Run Shell Script” action.
*   Check the “Show this action when the workflow runs.” This setting ensures that the Quick Action becomes accessible and can be linked to keyboard shortcuts.

**Accessing and Activating the Script:**

*   Once the Quick Action is set up, navigate to System Preferences > Keyboard > Shortcuts > Services > General.
*   Here, you should find the new Quick Action listed. Assign a keyboard shortcut to it. (Make sure you don’t use those shortcuts for something else or override them in different apps so it saves you the trouble)
*   Now, whenever you press the assigned shortcut, your script runs, changing the monitor alignment as programmed.

**Scripts for Monitor Alignment:**
==================================

**Horizontal Alignment Script:**

*   This script sets up your monitors side by side.
*   It first identifies the display IDs and then sets the primary display to a specific resolution and orientation.
*   The secondary display is placed to the right of the primary, with its position dynamically calculated based on the primary display’s width

`
export PATH="/usr/local/bin:$PATH" \
display\_ids=($(displayplacer list | grep "Persistent screen id:" | awk '{print $4}'))  \
PRIMARY\_DISPLAY=${display\_ids\[0\]}  \
SECONDARY\_DISPLAY=${display\_ids\[1\]} \ 
primaryWidth=$(displayplacer list | grep "$PRIMARY\_DISPLAY" | sed -n 's/.\*res:\\(\[0-9\]\*\\)x\[0-9\]\*.\*/\\1/p')  \
displayplacer "id:$PRIMARY\_DISPLAY res:1440x900 hz:60 color\_depth:8 enabled:true scaling:on origin:(0,0) degree:0" "id:$SECONDARY\_DISPLAY res:1920x1080 origin:($primaryWidth,-180)"
`

Keep in mind, when we set `origin:($primaryWidth, -180)` the purpose was to line up the monitors with the same bottom screen, that could be 0 or however you see fit for yourself

**Vertical Alignment Script:**

*   This script arranges your monitors vertically.
*   Similar to the horizontal script, it identifies the display IDs and adjusts the primary display.
*   The secondary display is placed above or below the primary, with its position dynamically calculated based on the primary display’s height.

`
export PATH="/usr/local/bin:$PATH"  
display\_ids=($(displayplacer list | grep "Persistent screen id:" | awk '{print $4}'))  
PRIMARY\_DISPLAY=${display\_ids\[0\]}  
SECONDARY\_DISPLAY=${display\_ids\[1\]}  
primaryHeight=$(displayplacer list | grep "$PRIMARY\_DISPLAY" | sed -n 's/.\*res:\[0-9\]\*x\\(\[0-9\]\*\\).\*/\\1/p')  
displayplacer "id:$PRIMARY\_DISPLAY res:1440x900 hz:60 color\_depth:8 enabled:true scaling:on origin:(0,0) degree:0" "id:$SECONDARY\_DISPLAY res:1920x1080 origin:(-280,-$primaryHeight)"
`

The same thing here with the `origin:(-280, -$primaryHeight)`, I set it like that so it fits perfectly in the middle with my screen but that could change based on the monitor, type of Macbook screen or settings.

**Implementing the Scripts in Automator:**

*   For each alignment (horizontal or vertical), create a separate Quick Action in Automator following the steps previously described.
*   Paste the respective script into the “Run Shell Script” action for each Quick Action.
*   Assign different keyboard shortcuts to each Quick Action for easy toggling between horizontal and vertical alignments.

**Running the Scripts:**

*   Once everything is set up, you can press the designated keyboard shortcuts to change the monitor alignment.
*   Ensure that the “Run Shell Script” options are correctly configured to allow execution.

**Visual Confirmation:**

*   Upon running each script, your monitors will rearrange into the specified alignment. The change should be immediate and noticeable as your screen adjusts.

By setting up two separate scripts and corresponding Quick Actions, you provide flexibility for users to switch between horizontal and vertical alignments quickly. This section of the blog will guide users through implementing and using these scripts effectively, enhancing their productivity and ease of use with multi-monitor setups.
