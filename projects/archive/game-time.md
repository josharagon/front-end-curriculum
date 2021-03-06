---
title: Game Time
length: 2 weeks
tags: javascript, jquery, canvas, svg, mocha, testing
---

## Abstract

Learn object oriented programming (OOP) principles by building a game that is playable in the browser.

<!-- link broken, nfoster, Mar-12-17 -->
<!-- This project is inspired by [Minicade](http://minica.de/). -->

## Goals

* Use OOP to drive the design of the game and the code
* Separate business-logic code from view-related code
* Create a robust test suite that thoroughly tests all functionality of a client-side application

### Restrictions

You can use any of the following JavaScript libraries:

* [jQuery](http://jquery.com/)
* [Mocha](http://mochajs.org/)
* [Moment.js](http://momentjs.com)
* [Numeral.js](http://numeraljs.com)

(Other libraries may be used *only* with instructor approval.)

**Nota bene**: We provide a [Game Time Starter Kit](https://github.com/turingschool-examples/game-time-starter-kit-FEm1) that has been preconfigured with webpack. You should use this starter kit.

## Game Choices

- Lights Out
- Snake
- Tron
- Othello/Reversi
- Frogger
- Missile Command

Rank your top three game choices and submit them to instructors by starting a direct message that includes both project partners and all module instructors. (Game requests sent to only one instructor, or missing the project partner, do not count.) Instructors will try to assign you a game in your top three choices, but may suggest a different one based on how many students choose a particular game and difficulty levels.

## Playability Features

We want your game to be as full-featured as possible. Make sure to include sufficient UX to allow the user to fully interact with the game. This would include:

* Indicate when the game is over and won or lost
* Allow the user to start a new game
* Display a score (if applicable)
* Include a clean UI surrounding the actual game interface itself (this might include instructions on how to play, a high score saved in localStorage, etc. Think of what would be most intuitive for your particular game.)
* Include multiple levels of difficulty

## Code organization

Your game should make use of at least two classes; the exact number will depend on which game you choose and your design choices.

You should use [inheritance](https://www.sitepoint.com/understanding-ecmascript-6-class-inheritance/) with your classes.
  - a parent class should have properties that might be shared by several other child classes
  - a parent class's properties and methods should be shared by all the child classes
  - a child classes should inherit those properties from the parent class

The game-time-starter-kit starts with a GamePiece class from which your other classes should inherit.

Each class should have its own file with the filename capitalized. The class should be capitalized as well. Only code that is a part of this class should be in this file.

i.e.
```
// Person.js
class Person {
  constructor (name) {
    this.name = name;
  }
}

// Villain.js
class Villain extends Person {
  constructor (name, skills) {
    super(name);
    this.skills = skills;
  }
}
```

## User Interface

The UI of the game should be clean, intuitive, and informative:
 - tell the user when the game is over (due to losing, winning, or completing the game)
 - tell the user the score or number of lives remaining (if applicable)
 - allow the user to start a new game

If your game uses the arrow keys, you should prevent the page from scrolling when the arrow keys are pressed.

## Testing

Each JavaScript file in your project should have its own test file
e.g.
if you have a `MasterChief.js` class file, all the tests for that class should be located in `MasterChief-test.js`

The test suite will test all functionality of the game (excepting anything touching the DOM):
* Class default properties
* Class methods
* Anything that updates class properties
* Class interactions (e.g. a ball colliding with a brick, a frog landing on a lilypad, a score or level incrementing, etc)
* etc

## Extensions

* Create multiple rounds of difficulty (consider increasing factors such as game speed, randomness of starting setup, etc)
* Create an AI player
* Get your game hosted on GitHub pages 
* Write a blog post on Medium detailing your experiences building a game with HTML5 Canvas and OOP

### Functional Expectations

* 4 - Application is fully playable and exceeds the expectations set by instructors. At least one extension is in place.
* 3 - Application is fully playable and exceeds the expectations set by instructors.
* 2 - Application has some missing functionality but no bugs or broken functionality.
* 1 - Application is unplayable due to lack of functionality or broken logic.

### User Interface

* 4 - The application is pleasant, logical, and easy to use. There are no holes in functionality and the application stands on its own to be used by the instructor _without_ guidance from the developer.
* 3 - The application has many strong pages/interactions, but a few holes in lesser-used functionality.
* 2 - The application shows effort in the interface, but the result is not effective. The evaluator has some difficulty using the application when reviewing the features in the user stories.
* 1 - The application is confusing or difficult to use.

### Testing

* 4 - Project has a running test suite that exercises the application at multiple levels. The test suite covers almost all aspects of the application and uses mocks and stubs when appropriate. ESLint shows 0 complaints.
* 3 - Project has a running test suite that tests multiple levels but fails to cover some features. All functionality is covered by tests. The application makes some use of integration testing. ESLint shows < 5 complaints.
* 2 - Project has sporadic use of tests at multiple levels. The application contains numerous holes in testing and/or many features are untested. ESLint shows 5+ complaints.
* 1 - There is little or no evidence of testing in this application. ESLint shows 10+ complaints.

### JavaScript Style & OOP

* 4 - Application has exceptionally well-factored code with little or no duplication. SRP (single responsibility principle) and DRY (don't repeat yourself) principles are utilized. There are _zero_ instances where an instructor would recommend taking a different approach. Application is organized into classes (and correctly uses inheritance) and there are no instances where instructor would suggest moving logic or data to another class. The business-logic code driving functionality is cleanly separated from rendering, view-related code.
* 3 - Application is thoughtfully put together with some duplication and no major bugs. Developer can speak to choices made in the code and knows what every line of code is doing. Application is organized into classes (and correctly uses inheritance) with some misplaced logic and no major bugs. Business-logic code is mostly separated from view-related code. Developer can speak to choices made in the code and knows what each line of code is doing.
* 2 - Your application has a significant amount of duplication and one or more major bugs. Application is organized into classes that do not display a good understanding of encapsulation, and logic is not well-divided. Developer cannot articulate what each line of code is doing. There are one or more major bugs.
* 1 - Your client-side application does not function. Developer writes code with unnecessary variables, operations, or steps that do not increase clarity. Application is not separated into classes, or methods and properties are illogically assigned to classes. Developer writes code with unnecessary variables, operations, or steps that do not increase clarity. Business-side logic and view-related code is not separated at all.


### Workflow

* 4 - The developer/team effectively uses Git branches and many small, atomic commits that document the evolution of their application with descriptive commit messages. The team displays good pairing practices (driver-navigator, dividing up work, etc) and utilizes some sort of planning tool (GitHub issues, Waffle, Trello, etc). The team develops a clear MVP (minimum viable product) and conducts a DTR (define the relationship). Both members contribute meaningfully to the application.
* 3 - The developer/team makes a series of small, atomic commits that document the evolution of their application. There are no formatting issues in the code base. The team conducts a DTR (define the relationship) and utilizes a planning tool and pairing practices. Both members contribute meaningfully to the application.
* 2 - The developer/team makes large commits covering multiple features that make it difficult for the evaluator to determine the evolution of the application. The team does not utilize a planning tool or equitable pairing practices. One or both members do not contribute meaningfully to the application.
* 1 - The developer/team committed the code to version control in only a few commits. The evaluator cannot determine the evolution of the application.
* 0 - The application was not checked into version control.
