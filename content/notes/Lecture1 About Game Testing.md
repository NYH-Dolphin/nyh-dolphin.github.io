---
title: "Lecture1 About Game Testing"
date: 2022-04-15
draft: false
type: "post"
tags: ["Game Testing"]
---

# Lecture1 About Game Testing

> http://testbook.gamedev.net/

## 1. Content of the Book

- Part I "About Game Testing"
  - Introduces the reader to game testing in terms of culture, philosophies, and the contribution testing makes to the final game release that everyone.
- Part II "Making Games"
  - Reveals how an individual contributes to the overall game project. This includes the **different kinds of roles and responsibilities** that are required of testers through various stages of the development and production of game software.
- Part III "Testing Fundamentals"
  - Introduces **testing concepts and practices from a formal software engineering approach**. These practices will boost your testing IQ. Tools and files included on the book’s CD will help you quickly produce useful test documents, capture important data, and analyze the measurements described in this section.
- Part IV "Testing Techniques"
  - **A set of tutorials on different methodologies for producing tests for your game.** Each can be used by itself, or in combination with the others. They are good for testing any portion of your game at any stage of development.
- Part V "More Effective"
  - Addresses ways to make the most out of your limited time and resources to reach new heights in the quantity and quality of the test you can produce and run.

## 2. Two Rules of Game Testing

### Don't Panic

#### The reason of Panic

Game project panic happens when you are

- **Unfamiliar**
  - Don't make a commitment right way
  - Based on your judgment
  - Get help from other people/website
- **Unprepared**
  - Expect the unexpected
  - Try to get to know more about the game code
  - Keep up with the industry
  - Get familiar with the ones you aren’t responsible for
- **Under pressure**
  - Pressure can come from any of three directions	
    - **Schedule** (calendar time to complete the project)
    - **Budget** (money to spend on the project)
    - **Headcount** (the quantity and types of people assigned to work on the game)
  - Examine the schedule, budget, and headcount available to you
  - Achieve the request by then scaling down what you would normally do so that it fits in your new triangle
  - Do the things that will have the most impact on meeting the request to the greatest extent possible
- **Unrested**
  - Make a checklist to use before and after the testing
  - Writing down some of the information as you go along
  - After testing is done, record pertinent results and facts
  - Automation
- **Nearsighted**
  - Familiar with the situation
  - Prepared to deal with if from practice/study/experience
  - Rested
  - Don't feel pressure to make up the deficit immediately

#### Checklist Template

<img src="/images/Lecture1 About Game Testing.assets/image-20220409141753359.png" alt="image-20220409141753359"  />

### Trust No One

#### Trust Issues

The very existence of testers on a game project is a result of trust issues, such as the following:

- The publisher doesn’t trust that your game will release on time and with the promised features, so they write contracts that pay your team incrementally based on demonstrations and milestones.
- The press and public don’t trust that your game will be as good and fun and exciting as you promise, so they demand to see screen shots and demos, write critiques, and discuss your work in progress on bulletin boards.
- Project managers don’t trust that the game code can be developed without defects, so testing is planned, funded, and staffed. This can include testers from a third-party QA house and/or the team’s own internal test department.
- The publisher can’t trust the development house testers to find every defect, so they may employ their own testers or issue a beta release for the public to try it out and report the defects they find.

#### Balancing Art

<img src="/images/Lecture1 About Game Testing.assets/image-20220409142640946.png" alt="image-20220409142640946" style="zoom:67%;" />	<img src="/images/Lecture1 About Game Testing.assets/image-20220409142709546.png" alt="image-20220409142709546" style="zoom:70%;" />

- Evaluate the basis of your testing plans and decisions
  - Test methods
  - Documenting
- Measuring and analyzing test results
  - The parts that you **trust the least**, the weak ones, will **need the most attention** in terms of testing, retesting, and analysis
  - The parts you can **trust the most**, the strong ones, will **require the least attention** from you

#### Word Games

- Don't always trust the people outside your test team, at the same time, don’t cross the boundary from being distrustful to turning hostile
  - You’ll be surprised how many bugs you will find by behaving opposite from the advice you get from other people about what should and should not be tested
- A general form of statements to watch out for is “**X happened, so (only/don’t) do Y.**”
  - “Only a few lines of code have changed, so don’t inspect any other lines.”
  - “The new audio subsystem works the same as the old one, so you only need to run your old tests.”
  - “We added foreign language strings for the dialogs, so just check a few of them in one of the languages and the rest should be okay too.”
  - “We only made small changes so don’t worry about testing [insert feature name here].”
  - “You can just run one or two tests on this and let me know if it works.”
  - “We’ve gotta get this out today so just ….”

#### Last Chance

- Examine your own tests and look for ways you can improve so you gain more trust in your own skills in finding defects
- Leave room to mistrust yourself just a little bit
- Remain open to suggestions from managers, developers, other testers, and yourself
- Always there are many reasons for late defects showing up
  - Introduced late, just before you found them
  - Bugs from earlier rounds of testing kept you from getting to the part of the game where the late defect was hiding
  - Natural that especially subtle problems might not be found until late in the project

### Summary

Panic results in

- Poor judgment and decision making
- Unreliable test results
- Too much emphasis on the short-term

Panic costs the project in

- Unnecessary rework
- Wasted effort
- Loss of confidence and credibility

Avoid panic by

- Recognizing when you need help, and getting it
- Preparing for the unexpected
- Relying on procedures
- Getting sufficient rest 

Don’t trust

- Hearsay
- Opinions
- Emotions 

Rely on

- Facts
- Results
- Experience

Test each game release as if

- It’s not the last one
- It is the last one

## 3. Being a Game Tester

### PLANo TV

This is an acronym: Play, Identify, Amplify, Notify, and optionally, Testify and Verify

<img src="/images/Lecture1 About Game Testing.assets/image-20220409145215961.png" alt="image-20220409145215961"  />

### Playing Games

As a game tester, you need to play game intentional

#### Eg. A portion of the character selection UI in Star Wars: Knights of the Old Republic for Xbox

1. Select New Game from the Main Menu.
   1. Check that the Male Scoundrel picture and title are highlighted.
   2. Check that the scoundrel character description is displayed correctly.
2. Scroll to the left using the D-Pad.
   1. Check that the Female Scoundrel picture and title are highlighted.
   2. Check that the scoundrel character description is unchanged. 
3. Scroll to the right using the D-Pad.
   1. Check that the Male Scoundrel picture and title are highlighted.
   2. Check that the scoundrel character description is unchanged.
4. Scroll to the right using the LEFT analog stick.
   1. Check that the Male Scout picture and title are highlighted.
   2. Check that the scout character description is displayed correctly.
5. Scroll to the left using the LEFT analog stick.
   1. Check that the Male Scoundrel picture and title are highlighted.
   2. Check that the scoundrel character description is displayed correctly. 
6. Scroll to the right using the RIGHT analog stick.
   1. Check that the Male Scoundrel picture and title are unchanged.
   2. Check that the scoundrel character description is unchanged.
7. Press the X button.
   1. Check that the Male Scoundrel picture and title are unchanged.
   2. Check that the scoundrel character description is unchanged.
8. Press the Y button.
   1. Check that the Male Scoundrel picture and title are unchanged.
   2. Check that the scoundrel character description is unchanged.
9. Press the B button.
   1. Check that the Main Menu screen is displayed with “New Game” highlighted.

#### Eg. Tina Armstrong’s special attacks in Dead or Alive 3

- Machine Gun Missile
- Triple Elbow 
- Combo Drop Kick 
- Turn Uppercut 
- Dolphin Uppercut 
- Knee Hammer 
- Leg Lariat 
- Front Step Kick 
- Crash Knee 
- Short Range Lariat 
- Elbow Suicide 
- Front Roll Elbow 
- Front Roll Kick 
- Flying Body Attack

### Identifying Bugs

#### Tags of test

- "**Passes**"
  - Successfully pass the test
- "**Fails**"
  - Fail to pass the test
- "**Blocked**"
  - An existing problem keeps you from getting to other parts of the test
- "**Not Available**"
  - The part you are supposed to test has not been included in the version of the game you were given to test
  - The developers are still in the process of getting all the game elements put together

#### Judger and Perceiver

<img src="/images/Lecture1 About Game Testing.assets/image-20220409151122028.png" alt="image-20220409151122028" style="zoom: 80%;" />

- Judger
  - prefer a structured, ordered, and fairly predictable environment, where you can make decisions and have things settled
  - good at following step-by-step instructions, running through a lot of tests, and finding problems in game text, the user manual, and anywhere the game is inconsistent with historical facts
  - Judgers will verify the game’s "authenticity"
  - A Judger may not do steps or notice problems that aren’t in the written tests
- Perceiver
  - prefer to experience as much of the world as possible. You like to keep your options open and are most comfortable adapting
  - tends to wander around the game, come up with unusual situations to test, report problems with playability, and comment on the overall game experience
  - Perceivers will verify its "fun-ticity"
  - A Perceiver may miss seeing problems when running a series of repetitive tests

### Amplifying Problems

#### Early Bird

Check the newly updated things

- New levels, characters, items, cut scenes, and so on as soon as they are introduced
- New animations, lighting, physics, particle effects, and so on
- New code that adds functionality or fixes defects
- New subsystems, middleware, engines, drivers, and so on
- New dialog, text, translations, and so on
- New music, sound effects, voice-overs, audio accessories, and so on

#### Places Everyone

Find defects in more places within the game by looking for the following

- All of the places in the game where the same faulty behavior can be activated
- All of the places in the code that call the defective class, function, or subroutine
- All of the game functions that use the same defective item, scene, and so on
- All of the items, levels, characters, and so on that have a shared attribute with the faulty one (for example, character race, weapon type, levels with snow, and so on)

And then, use this two-step process to increase the frequency of the defect:

- Eliminate unnecessary steps to get the defect to appear.
- Find more frequent and/or more common scenarios that could include the remaining essential step

### Notify the Team

You need to record that information and notify the developers about it

`DevTrack` is one of the defect tracking and management tools used in the game industry

[Best Project Task Management Software | Techexcel](https://techexcel.com/products/devtrack/)

<img src="/images/Lecture1 About Game Testing.assets/image-20220409152558979.png" alt="image-20220409152558979" style="zoom:80%;" />	

#### Describe

- Provide information details that help narrow down the problem
  - **what**
  - **who**
  - **where**
  - **when**
  - **how**
- **Title**
  - what
  - other option
- **Description**
  - describe other information option left
  - describe how you were able to remedy the situation
  - or provide a step-by-step description of how you found it
- It helps the project leaders evaluate the importance of fixing the bug
- It gives the developers clues about how the problem happened and how they might go about fixing it

#### Prioritize

Rank the defect according to its importance, as defined for each choice

- **Urgent**: stops or aborts the game in progress without any way to recover and continue the game
- **High**: causes some severe consequence to the player
- **Medium**: cause noticeable problems, but probably do not impact the player in terms of rewards or progress
- **Low**: very minute defects that don’t affect gameplay, those that occur under impossible conditions, or those that are a matter of personal taste

#### Pick a Type

- **CR: Change Request**
  - something that has a larger scope, such as redoing the collision detection mechanism in the game
- **DP: Documentation Problem**
  - looking for consistency with how the actual game functions, important things that are left out, or production errors such as missing pages, mislabeled diagrams, and so on
- **DR: Discrepancy Report**
  - the game is simply not working the way it is supposed to
- **FE: Feature Enhancement**
- **MI: Minor Improvement**
- **NF: New Feature**
  - eg
    - An idea for the sequel
    - An optimization to make for the next platform you port the game to
    - Adding support for a brand new type of controller
    - A feature or item to make available for download after the game ships
- **TP: Third Party**
  - introduced by software or hardware that your team does not produce

#### Be Helpful

Include any other artifacts or information that might be of help to anyone trying to assess or repair the problem

- Server logs
- Screen shots
- Transcripts from the character’s journal
- Sound files
- Saved character file
- Videotape recording (including audio) of the events leading up to and including the bug
- Traces of code in a debugger
- Log files kept by the game platform, middleware, or hardware
- Operating system pop-ups and error codes
- Data captured by simulators for mobile game environments, such as BREW®, Java, Palm OS™, or Microsoft® Windows® CE

### Testify to Others

- providing more details about how to reproduce the bug and why it is important to fix
- balancing ownership of the defect with knowing when to let go
  - find stuff and report it
  - other people are usually designated to take responsibility for properly processing and repairing the defect
  - Don't take it personally if your defect doesn't get fixed first, or other people don t get as excited about it as you do

### Verify the Fix

- help developers reproduce the bug
- run experiment for them
- re-test after they think the bug is fixed

## 4. Why Testing is Important

### Who Cares?

Because games get made wrong, and you need to identify mechanisms or patterns that describe how games get made wrong

- It s important to console providers because they require certain quality standards to be met before they will allow a title to ship for their box. 
- Mobile game testing is important to handset manufacturers and wireless carriers in order to get approved for their devices and networks 
- Development team rely on testers to find problems in the code
- The contractual commitments and complex nature of the software required to deliver a top-notch game

### Defect Typing

#### Function

A Function error is one that affects a game capability or how the user experiences the game.

The code providing this function is missing or incorrect in some or all instances where it is required.

#### Assignment

It is the result of incorrectly setting or initializing a value used by the program or when a required value assignment is missing.

Many of the assignments take place at the start of a game, a new level, or a game mode.

#### Checking

The code fails to properly validate data before it is used.

This could be a missing check for a condition or the check is improperly defined. Some examples of improper checks in C code would be the following.

#### Timing

The management of shared and real-time resources.

Some processes may require time to start or finish, operations that depend on that data shouldn’t be prevented until completion of the dependent process.

Information is flying around between players and the game server(s), this information has to be reconciled and handled in the proper order or the game behavior will be incorrect.

#### Build/Package/Merge

Result of mistakes in using the game source code library system, managing changes to game files, or identifying and controlling which versions get built.

#### Algorithm

Efficiency or correctness problems that result from some calculation or decision process

#### Documentation

In the fixed data assets that go into the game, include

- Text
- Dialogs
- User interface elements (labels, warnings, prompts, etc.)
- Help text
- Instructions
- Quest journals
- Audio
- Sound effects
- Background music
- Dialog (human, alien, animal)
- Ambient sounds (running water, birds chirping, etc.)
- Celebration songs 
- Video
- Cinematic introductions
- Cut scenes
- Environment objects 
- Level definitions
- Body part and clothing choices
- Items (weapons, vehicles, etc.)

#### Interface

Information is being transferred or exchanged.

Inside the game code, Interface defects occur when something is wrong in the way one module makes a call to another. If the parameters passed on somehow don’t match what the calling routine intended, then undesired results occur.

-  Calling a function with the wrong value of one or more arguments
- Calling a function with arguments passed in the wrong order 
- Calling a function with a missing argument 
- Calling a function with a negated parameter value 
- Calling a function with a bitwise inverted parameter value 
- Calling a function with an argument incremented from its intended value 
- Calling a function with an argument decremented from its intended value

