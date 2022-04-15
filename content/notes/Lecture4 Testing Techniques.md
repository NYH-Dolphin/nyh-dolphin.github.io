---
title: "Lecture4 Testing Techniques"
date: 2022-04-15
draft: false
type: "post"
tags: ["Game Testing"]
---

# Lecture4 Testing Techniques

## 1. Combinatorial Testing

Pairwise combinatorial testing is a way to find defects and gain confidence in the game software while keeping the test sets small relative to the amount of functionality they cover

- that each value you use for testing needs to be combined at least once with each other value of the remaining parameters

### Parameters

Parameters are the individual elements of the game that you want to include in your combinatorial tests

- Game events
- Game settings
- Gameplay options
- Hardware configurations
- Character attributes
- Customization choices

### Values

Values are the individual choices that are possible for each parameter

- number
- text
- list
- â€¦

### Defaults

Consider whether or not default values should be used in your tests.

### Enumerations

Many choices in a game are made from a set of distinct values or options that do not have any particular numerical or sequential relationship to one another.

Regardless of the number of unique choices (team, car, fighter, weapon, song, hairstyle, and so on), **each one should be represented somewhere in your tests**.

### Ranges

Three particular values tend to have special defect-revealing properties

- **Zero**
  - A loop may prematurely exit or may always do something once before checking for zero
  - Confusion between starting loop counts at 0 or 1
  - Confusion with arrays or lists starting at index 0 or 1
  - 0 is often used to represent special meaning, such as to indicate an infinite timer or that an error has occurred
  - 0 is the same value as the string termination (NULL) character in C
  - 0 is the same value as the logical (Boolean) False value in C
- **Minimum & Maximum**
  - Time
  - Distance
  - Speed
  - Quantity
  - Size
  - Bet amount

### Boundaries

Some of the boundaries to test may be physically rendered in the game space

Dig deep into the rules of the game to identify hidden or implied boundaries

- Town, realm, or city borders
- Goal lines, sidelines, foul lines, and end lines on a sports field or court
- Mission or race waypoints
- Start and finish lines
- Portal entrances and exit
- Mission, game, or match timers
- The speed that a character or vehicle can achieve
- The distance a projectile can travel
- The distance at which graphic elements become visible, transparent, or invisible

### Constructing Tables

For a **pairwise combinatorial table**, it’s only necessary to **combine each value of every parameter with each value of every other parameter at least once somewhere in the table**.

Create pairwise tables of any size for your game tests using the following short and simple process. These steps may not always produce the optimum (smallest possible) size table, but you will still get an efficient table

1. Choose the parameter with the highest dimension
2. Create the first column by listing each test value for the first parameter N times, where N is the dimension of the next-highest dimension parameter
3. Start populating the next column by listing the test values for the next  parameter
4. For each remaining row in the table, enter the parameter value in the new column that provides the greatest number of new pairs with respect to all  of the preceding parameters entered in the table. If no such value can be found, alter one of the values previously entered for this column and resume this step
5. If there are unsatisfied pairs in the table, create new rows and fill in the  values necessary to create one of the required pairs. If all pairs are satisfied, then go back to step 3
6. Add more unsatisfied pairs using empty spots in the table to create the most new pairs. Go back to step 5
7. Fill in empty cells with any one of the values for the corresponding column (parameter)

### Combinatorial Tools

James Bach has made a tool available to the public at www.satisfice.com/tools.shtml that handles Combinatorial Testing

<img src="/images/Lecture4 Testing Techniques.assets/image-20220411210913214.png" alt="image-20220411210913214" style="zoom:67%;" />	

## 2. Test Flow Diagrams

**Test flow diagrams (TFDs)** are **graphical models** representing game behaviors from the player’s perspective.

### TFD Elements

- A TFD is created by assembling various drawing components called elements
- These elements are drawn, labeled, and interconnected according to certain rules

#### Flows

Flows are drawn as **a line connecting one game state to another**, with **an arrowhead indicating the direction** of flow

<img src="/images/Lecture4 Testing Techniques.assets/image-20220411212235330.png" alt="image-20220411212235330" style="zoom:50%;" />	

-  During testing, you do **what is specified by the event** and then check for **the behavior specified by both the action and the flow’s destination state**

#### Events

Events are **operations initiated by the user, peripherals, multiplayer network, or internal game mechanisms**

- Picking up an item, selecting a spell to cast, sending a chat message to another player, and an expiring game timer are all examples of events
- Three factors should be considered for including a new event
  - Possible interactions with other events
  - Unique or important behaviors associated with the event
  - Unique or important game states that are a consequence of the event
- **Only one event can be specified on a flow**, but **multiple operations can be represented by a single event**
- Events may or may not cause a transition to a new game state

#### Actions

An action exhibits **temporary or transitional behavior** in response to an event

- Actions can be perceived through human senses and gaming platform facilities, including sounds, visual effects, game controller feedback, and information sent over a multiplayer game network
- **Only one action can be specified on a flow**, but **multiple operations can be represented by a single action**

#### States

States represent persistent game behavior and are re-entrant

- As long as you don’t exit the state you will continue to observe the **same behavior**, and each time you **return to the state you should detect the exact same behavior**
- A state is drawn as a “bubble” with a unique name inside
- Each state must have **at least one flow entering and one flow exiting**

#### Primitives

**Events**, **actions**, and **states** are also referred to as primitives

#### Terminators

Terminators are special boxes placed on the TFD that indicate **where testing starts and where it ends**

- two terminators should appear on each TFD
  - IN box
  - OUT box

### TFD Design Activities

 Go through three stages of activities: **preparation**, **allocation**, and **construction**

#### Preparation

Collect sources of game feature requirements

- Identify the requirements that fall within the scope of the planned testing, based on your individual project assignment or the game’s test plan

#### Allocation

Estimate the number of TFDs required and map game elements to each

Separate large sets of requirements into smaller chunks, try to cover related requirements in the same design

- test various abilities provided in the game
- map situations or scenarios to individual TFDs with a focus on specific achievements

#### Construction

Model game elements on their assigned TFDs using a **“player’s perspective.”**

### TFD Example

#### Preparation

The ability to **pick up a weapon and its ammo** while the game properly **keeps track of your ammo count and performs the correct audible and visual effects**

#### Allocation

This is a simple feature test, and it's only need one TFD

#### Construction

| Diagram                                                      | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214138198.png" alt="image-20220411214138198" style="zoom:80%;" /> | Start with an **IN box**, followed by a flow to the **first state<br />**Jump right to the point in the game where you want to start doing things |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214303317.png" alt="image-20220411214303317" style="zoom:80%;" /> | Model what happens when the player does something in this situation<br />**Event**: Find a gun and pick it up<br />**Action**: <br />A gun appears in your inventory<br />your character is shown holding the gun<br />a crosshair now appears at the center of the screen<br />playing the sound of a weapon being picked up<br />identifying the weapon the display |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214456186.png" alt="image-20220411214456186" style="zoom:80%;" /> | Event: Get some Ammo and pick them up<br />Action: Ammo related effects |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214510563.png" alt="image-20220411214510563" style="zoom:80%;" /> | …                                                            |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214520824.png" alt="image-20220411214520824" style="zoom:80%;" /> | …                                                            |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214538713.png" alt="image-20220411214538713" style="zoom:80%;" /> | …                                                            |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214603921.png" alt="image-20220411214603921" style="zoom:80%;" /> | …                                                            |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214703326.png" alt="image-20220411214703326" style="zoom:80%;" /> | …                                                            |
| <img src="/images/Lecture4 Testing Techniques.assets/image-20220411214728836.png" alt="image-20220411214728836" style="zoom:80%;" /> | …                                                            |

### Data Dictionary

The **data dictionary** provides **detailed descriptions for each of the uniquely named primitive elements** in your TFD collection

### TFD Path

A test path is a series of flows, specified by the flow numbers in the sequence in which they are to be traversed. Paths begin at the IN state and end at the OUT state

- A path defines an individual test case that can be “executed” to explore the game’s behavior
- Many paths are possible for a single TFD

#### Minimum Path Generation

This strategy is designed to produce the **smallest number of paths that will end up covering all of the flows in the diagram** (Graph Problem)

#### Baseline Path Generation

Baseline path generation begins by establishing as direct a path as possible from the IN Terminator to the OUT Terminator that travels through as many states without repeating or looping back (Graph Problem)

#### Expert Constructed Paths

Expert constructed paths are simply paths that a test or feature **“expert” traces based on her knowledge of how the feature is likely to fail** or where she needs to establish confidence in a particular set of behaviors

- Repeat a certain flow or sequence of flows in combination with other path variations
- Create paths that emphasize unusual or infrequent events
- Create paths that emphasize critical or complex states
- Create extremely long paths, repeating flows if necessary
- Model paths after the most common ways the feature will be used

### To TFD or Combinatorial table?

<img src="/images/Lecture4 Testing Techniques.assets/image-20220411215504543.png" alt="image-20220411215504543" style="zoom:80%;" />	

## 3. Cleanroom Testing

Cleanroom testing is applied to the problem of why customers find problems in games after they have been through thousands of hours of testing before being released.

**Cleanroom test produces tests that play the game the way players will play it.**

### Usage Probabilities

Usage probabilities, also referred to as usage frequencies, tell testers how often game functions should be used in order to realistically mimic the way customers will use the game.

#### Mode-Based Usage

Game usage can change based on which mode the player is using such as single player, campaign, multiplayer, or online

#### Player-Type Usage

Another factor that influences game usage is the classification of four multi-user player categories described

- **Achiever**: complete game goals, missions, and quests
- **Explorers**: finding out what the game has to offer
- **Socializer**: use the game as a means to role play and get to know other players
- **Killer**: getting the best of other players
- **Casual gamer**: Sticks mostly to functions described in the tutorial, user manual, and on-screen user interface
- **Hard-core gamer**: Uses function keys, macros, turbo buttons, and special input devices such as joysticks and steering wheels
- **Button Masher**: Values speed and repetition over caution and defense
- **Customizer**: Uses all of the game’s customization features and plays the game with custom elements
- **Exploiter**: Always looking for a shortcut

### Inverted Usage

Inverted usage can be applied when you want to emphasize the less frequently used functions and behaviors in the game

- This creates a usage model that might reflect how the game would be used by people trying to find ways to exploit or intentionally crash the game for their own benefit

#### Calculating Inverted Usage

1. Calculate the reciprocal of each usage probability for a test parameter (combinatorial) or for all paths exiting a state (TFDs).
2. Sum the reciprocals.
3. Divide each reciprocal from step 1 by the sum of the reciprocals calculated in step 2. The result is the inverted probability for each individual usage value.

## 4. Test Trees



## 5. Play Testing and Ad Hoc Testing

Focuses on a more chaotic, unstructured—yet no less crucial—approach to game testing

### Ad hoc Testing

Ad hoc testing, sometimes referred to as “general” testing, describes **searching for defects in a less structured way**

#### Free Testing

Allows the professional game tester to “depart from the script” and improvise tests on-the-fly

This method can include the following

- Assigning members of the multiplayer team to play through the single-player campaign
- Assigning campaign testers to skirmish (or multiplayer) mode
- Assigning the config/compatibility/install tester to the multiplayer team
- Assigning testers from another project entirely to spend a day (or part of a day) on your game
- Asking non-testers from other parts of the company to play the game (see the section “Play Testing” later in this chapter)
- Performing ad hoc testing early will quickly help to reveal any lingering  deficiencies in your test plans, combinatorial tables, and test flow diagrams  (see the following sidebar)

Before you begin, **you should have a goal**. This goal can be very simple, but it must be explicit. 

- How far can I play in story mode?
- Can I play a full game by making only three-point shots?
- Is there a limit to the number of turrets I can build in my base?
- Can I deviate from the strategy suggested in the mission briefing and still win the battle?
- Is there anywhere in the level I can get my character stuck in the geometry?

The following are but a few of the common pitfalls you should **avoid** when free testing

- Competing with other testers in multiplayer games
- Competing against the AI (or yourself) in single-player games
- Spending a lot of time testing features that may be cut
- Frequently testing the most popular features of the game
- Spending a disproportionate amount of time testing features that are infrequently used

#### Directed Testing

Intended to solve a specific problem or find a specific solution

The simplest form of directed testing answers a very specific question, such as

- Does the new compile work?
- Can you access all the characters?
- Are the cut-scenes interruptible?
- Is saving still broken?

The Scientific Method

1. Observe some phenomenon
2. Develop a theory—a hypothesis—as to what caused the phenomenon
3. Use the hypothesis to make a prediction; for example, if I do this, it will happen again
4. Test that prediction by retracing the steps in your hypothesis
5. Repeat steps 3 and 4 until you are reasonably certain your hypothesis is true

Find a crash

1. Review your notes: Quickly jot down any information about what you were doing when the defect occurred, while it’s still fresh in your mind
2. Process all this information and make your best educated guess as to what specific combination and order of inputs may have caused the crash
3. Read over the steps in your path until you are satisfied with them
4. Reboot your computer, restart the game, and retrace your steps, find whether you recurrence the crash

### Play testing

Play testing describes playing the game to test for such **subjective qualities as balance, difficulty, and the often-elusive “fun factor.”**

Does the game work well?

- Is the game too easy?
- Is the game to hard?
- Is the game easy to learn?
- Are the controls intuitive?
- Is the interface clear and easy to navigate?
- Is the game fun?

#### A Balancing Act

Balance can also refer to a state of rough equality between different competing units in a game

- Orcs vs. humans
- Melee fighters vs. ranged fighters
- The NFC vs. the AFC
- The Serpent Clan vs. the Lotus Clan
- Sniper rifles vs. rocket launchers
- Rogues vs. warlocks
- Purple triangles vs. red squares
