---
title: "Lecture5 More Effective Testing"
date: 2022-04-15
draft: false
type: "post"
tags: ["Game Testing"]
---

# Lecture5 More Effective Testing

## 1. Defect Triggers

Orthogonal Defect Classification (ODC) includes a set of Defect Triggers to categorize the way defects are caused to appear. These same triggers can be used to classify tests as well as defects. Test suites that do not account for each of the triggers will be incapable of revealing all of the defects in the game.

### Operating Regions

Game operation can be broken down into four stages

<img src="/images/Lecture5 More Effective Testing.assets/image-20220414191646144.png" alt="image-20220414191646144" style="zoom:67%;" />	

- Pre-Game
- Game Start
- In-Game
- Post-Game

#### Pre-Game Operating Region

- Console: Inserting the game disk
- PC/Mobile phones: Before start the game app

In each of these cases, the user can change settings and do things with the device that potentially impact the subsequent operation of your game.

#### Game Start Operating Region

The Game Start region accounts for operations that are performed from the time the player starts the game until the time the game is actually ready to be played.

- provide an introduction
- highlights of the game’s features
- displaying the “loading” progress
- performs activities that are essential to the proper operation of the game but are not visible to the player
- in a “ready” state, during which it is waiting for the player to hit a button or key in order to enter the game

#### In-Game Operating Region

The In-Game region covers all of things you could possibly do when playing the game

- some functions can only be performed once during the course of the game
- others can be repeated throughout the game

#### Post-Game Operating Region

End the game or a gaming session

- Quitting without saving requires less processing than when saving
- Games played on portable devices can be ended by turning off the device
- Story-based games treat the user to a final cinematic sequence and roll credits when users reach the end of the story.

### The Triggers

Six Defect Triggers span the four game operating regions.

<img src="/images/Lecture5 More Effective Testing.assets/image-20220414194428257.png" alt="image-20220414194428257" style="zoom:67%;" />	

#### The Configuration Trigger

- Pre-Game Region
  - device or environment settings
    - game platform software versions
    - date and time
    - screen resolution
    - system audio volume
    - operating system version
    - patches
    - language setting
  - external devices
    - Game controllers
    - keyboards
    - mouse
    - speakers
    - monitors
    - network connections
    - headsets
- In-Game Region
  - Disconnecting one or more devices during gameplay
    - Connecting a device could be done to correct an accidental disconnection

#### The Startup Trigger

The Startup trigger is utilized by attempting operations while some game function is in the process of starting up or immediately after that while code values and states are in their initial conditions.

- Loading please wait…
- Particular code vulnerabilities exist during the startup period
  - Code variables are being initialized
  - Graphics information is being loaded, buffered, and rendered
  - Information is read from and/or written to disk
- Game Start Region
  - user-initiated
  - caused by the game platform

#### The Exception Trigger

Special portions of the game code are exercised by the Exception trigger.

- In-Game Region
  - Some exceptions are under the control of the game
  - Other exceptions are caused by external conditions that are not under the control of the game software
    - network connection problems

#### The Stress Trigger

The Stress trigger tests the game under extreme conditions.

- conditions imposed on
  - hardware resources
  - software resources
  - memory
  - screen resolution
  - disk space
  - file size
  - network speed 

#### The Normal Trigger

This refers to using the game apart from any stress, configuration, or exception conditions, kind of like the way you would script a demo or show how the game should be played in the user manual.

- demonstrates that the game functions the way it is supposed to

#### The Restart Trigger

Restart trigger classifies a failure that occurs as a result of quitting, ending the game, turning off the game device, ejecting the game disk, or terminating the game’s operation in any other way

- Post-Game Region
  - save vital information before allowing you to exit a game scenario, mission, or battle in progress
  - When ending the game, some information needs to be remembered and some forgotten

## 2. Game Test Automation

### Can Automated Testing Work for Me?

<img src="/images/Lecture5 More Effective Testing.assets/image-20220414201303205.png" alt="image-20220414201303205" style="zoom: 67%;" />	

The potential advantages of automating game testing are 

- Improved game reliability
- Greater tester and machine efficiency
- Greater **consistency of results**
- Repetition, repetition, repetition
- Faster testing
- The ability to **simulate tremendously large numbers of concurrent players for stress and load testing without having to use large pools of human tester**

#### Production Costs

-  hiring new developers to write additional lines of code into a game to support automation
- test code may not work right and comes with its own set of bugs

#### Reusability

- Reusability of automated test code from one game to another
- changes could prevent some or all of the automated test code from working with the new title

#### Human Factor

- Hard to find more of the playability, balance, and “fun” issues in the game

#### Scale Matters

- large-scale automation will involve a fundamental shift in the way the entire company operates
- large-scale automation is a long-term investment that may not pay off in the first few games you create once the system is in place
- test automation is a form of software development

#### Great Expectations

- the fact that automatic testing tools are being used can lead managers to have unrealistic expectations about lower production costs, reduced development times, and so on
- it is vital that you set management expectations accurately and reasonably when proposing to automate even a small part of your testing process

#### Infrastructure

- build a solid infrastructure onto which the automatic testing programs can be placed, selected, and tracked

#### Team Work

- Still, you are likely to need a new game team structure that can focus on the needs of creating and rolling out your test automation program.
- It will be vital for the “automatons” to remain uninvolved in the day-today issues of either the game code development or manual testing.

#### Maturity Test

- Is your test department a mature, well-developed organism comprised of well-trained individuals with a clear testing system? 

### Types of Games to Automate

Some games are better suited to full-scale test automation than others

#### Massively Multiplayer Games

- Have a high number of repetitive actions
- Must simulate a very large number of players connected at once
- Are synchronized among a large number of servers and clients simultaneous
- Eg. server load, picking up and dropping any given object thousands of times, or entering, leaving, and re-entering rooms thousands of times

#### Other Types of Games

- Level-based games
  - sequences of play tested by well-written scripts that imitate a player in a certain pattern
  - Automatically test each combination of button press or menu item selection can significantly increase the early detection of bugs in the game code and lessen the chance that any bug will persist to retail release of the game
- Mobile Phone games
  - A developer can be required to have any given game run on many tens or hundreds of mobile phone handsets over the life of the title
  - Automating the testing of game screen displays, key press actions, and menu selections for each phone is advantageous over purely manual testing

### Five Habits of Effective Automated Testing

#### Filter Automated Test Results

Filter Automated Test Results should include both human and automated filters to ensure that **the same bug is not being reported over and over**, or that **trivial bugs are being passed on through the system and accorded the same level of priority as important bugs**

#### What Happens in Testing Stays in Testing

For very practical reasons, you definitely want to ensure that **all hooks and additional code you’ve added to your game code for testing purposes are removed prior to launch**\

Keep the game code **clean** for launch right from the early stages of development by ensuring that any comments placed in the code or any functions that are introduced for testing (infinite life, invulnerability, hooks for automatic test scripts, and so on) **can be easily and reliably removed.**

- consider whether such code or hooks can be placed in separate DLL files
- clearly label test files separately from the release version

#### Manage Build

Poor build management can also lead to immense **confusion** with automated scripts if the changes made in a given build should have been accompanied by revised test hooks written into the new game code or revised test programs and tools written to accommodate the new changes in the code

#### Test Game Code Prior to Committing

Building into your system the requirement that every **programmer must submit his or her new code for testing before it can be committed** is just good sense

#### Integrate Testing with Building

All new code gets submitted for testing before a coder can make a commit

Ensure your team is working on the same build version and that at any given time the build version is as stable and bug-free as possible

- At least once a day, automate the creation of a build that also coordinates all new and existing assets and executables. Automated testing can be used to ensure that new assets do not break the existing code and to test new code snippets, audio, and so on
- Make sure the system embeds version numbers and any other meta-data that might prove useful in the smooth running of the development cycle, such as asset and code source data and so on
- Make sure the system runs automated regression tests
- Once a day, have someone run the latest version of the game to check for  obvious errors the automated system may have missed

### Cost Analysis

Some of the predictable fixed costs when converting to automation are as follows

- Additional hardware (or at the least upgrades to existing hardware)
- Middleware and tool licenses (you are likely to need at least some)
- Scripting tool creation
- New game code to add hooks, framework, and so on for automation
- Tool training
- Management software and support
- Additional team members who are devoted to automation

You will face some variable costs, too:

- Test case implementation
- Test case designs specific to automation
- Maintenance
- Results analysis
- Defect reporting
- Night-time system runtime

## 3. Capture/Playback Testing

