# Agile Development, Git, GitHub, and Continuous Integration

## Video Goal

In this lesson, we connect several tools and practices that will be used throughout the **DungeonForge** project.

The goal is to understand:

* What **Agile development** is and why we use it
* How **user stories** turn requirements into manageable work
* How **Git** provides version control
* How **GitHub** provides collaboration and project-management tools around Git
* How **GitHub Projects** can organize Agile work
* How **GitHub Actions** can automate development tasks
* What **Continuous Integration (CI)** means
* How all of these pieces work together during the semester

The important idea is:

> **Agile is the development process; Git and GitHub provide the collaboration and version-control machinery; CI provides automated feedback that helps us maintain the quality of the code as the project evolves.**

---

# 1. What Is Agile Development?

**Agile** is an approach to software development based on building software **incrementally and iteratively**.

Instead of trying to design and build the entire game before anyone can play it, we continuously:

```text
Plan
  ↓
Build
  ↓
Test
  ↓
Review
  ↓
Improve
  ↓
Repeat
```

For DungeonForge, this means the game will grow throughout the semester.

We might begin with:

```text
Player
  ↓
Move around a dungeon
```

and gradually add:

```text
Combat
Items
Enemies
Inventory
Rooms
Spells
Saving
etc.
```

Each increment gives us something that works before we move on to the next feature.

---

# 2. Why Agile?

Large software projects are difficult because requirements, designs, and assumptions change as we learn more.

Agile embraces this reality.

Instead of asking:

> "How do we build the entire game correctly right now?"

we repeatedly ask:

> **"What valuable piece of the game should we build next?"**

This allows us to:

* Deliver functionality incrementally
* Get feedback early
* Discover design problems sooner
* Adapt to changing requirements
* Keep the project manageable

---

# 3. User Stories Connect Requirements to Development

Agile commonly expresses functionality as **user stories**.

A user story describes a feature from the perspective of someone using the system.

A common format is:

> **As a [type of user], I want [something], so that [reason].**

For DungeonForge:

> **As a player, I want to move my character through a dungeon so that I can explore the game world.**

This gives us three important pieces:

```text
WHO?
The player

WHAT?
Move through the dungeon

WHY?
To explore the game world
```

The story describes **what the software should accomplish**, without dictating exactly how we should implement it.

---

# 4. From User Story to Work

A user story is larger than a single line of code.

For example:

> **As a player, I want to move my character through a dungeon so that I can explore the game world.**

might require:

```text
Create Player class
Create Room class
Represent player location
Implement movement
Prevent invalid movement
Display the current room
Write tests
```

Agile gives us a way to organize this work without losing sight of the feature the player actually needs.

The relationship is:

```text
User Story
    ↓
Tasks
    ↓
Code
    ↓
Tests
    ↓
Working Feature
```

---

# 5. Agile Work Happens in Small Increments

Rather than attempting to implement everything at once, we break the project into manageable pieces.

For example:

```text
DungeonForge
│
├── Player movement
├── Dungeon rooms
├── Combat
├── Inventory
├── Items
├── Enemies
└── Character abilities
```

Each feature can be developed, tested, and integrated independently.

This is particularly important for a semester-long project.

> **The goal is to keep the game working while continuously adding functionality.**

---

# 6. Where Git Fits

**Git** is a **version control system**.

It records changes to our source code over time.

Instead of having:

```text
DungeonForge-final
DungeonForge-final2
DungeonForge-final-really-final
DungeonForge-final-fixed
```

Git maintains a history of the project.

Conceptually:

```text
Commit
   ↓
Commit
   ↓
Commit
   ↓
Commit
```

Each commit represents a point in the project's history.

Git allows us to:

* Track changes
* See what changed
* Create branches
* Experiment safely
* Return to previous versions
* Combine work from different developers

---

# 7. Where GitHub Fits

**Git is the version-control system.**

**GitHub is a platform built around Git.**

GitHub provides a place where the team can collaborate around the repository.

For DungeonForge, GitHub can provide:

```text
Git Repository
      +
Issues
      +
Pull Requests
      +
Projects
      +
Actions
```

These tools allow the source code, development work, planning, discussion, and automation to exist within one development environment.

---

# 8. GitHub Projects and Agile

**GitHub Projects** can provide the visual organization for our Agile workflow.

For example:

```text
Backlog
   ↓
To Do (Current Sprint backlog)
   ↓
In Progress
   ↓
In Review
   ↓
Done
```

A user story or task can move through these stages as development progresses.

For example:

```text
[Player can move]
       ↓
    Backlog
       ↓
     To Do
       ↓
   In Progress
       ↓
     Review
       ↓
      Done
```

This gives the team a visible representation of the current state of the project.

The important distinction is:

> **Agile defines how we organize and manage development; GitHub Projects gives us a tool for visualizing and managing that work.**

---

# 9. Git Branches Connect Work to Features

Developers generally should not make every change directly to the main branch.

Instead, a feature can be developed on its own branch:

```text
main
 │
 ├── feature/player-movement
 │
 ├── feature/inventory
 │
 └── feature/combat
```

For example:

```text
User Story
    ↓
Feature Branch
    ↓
Code
    ↓
Commit
    ↓
Pull Request
    ↓
Review
    ↓
main
```

This creates a controlled path for changes to enter the main codebase.

---

# 10. Pull Requests

A **Pull Request (PR)** is a request to merge changes into another branch, commonly `main`.

For example:

```text
feature/player-movement
          │
          │ Pull Request
          ▼
         main
```

A PR provides an opportunity to:

* Review the code
* Discuss the implementation
* Run automated tests
* Identify problems
* Decide whether the change is ready to merge

This fits naturally into an Agile workflow because the team can review work as features are completed rather than waiting until the entire project is finished.

---

# 11. Continuous Integration

Now we connect the development process to **Continuous Integration**, or **CI**.

CI means that changes are **frequently integrated into the shared codebase and automatically checked**.

The basic idea is:

```text
Developer
    ↓
Commit / Push
    ↓
GitHub / Pull
    ↓
Automated Build
    ↓
Automated Tests
    ↓
Feedback
```

Instead of relying entirely on a developer remembering to test everything manually, the project automatically checks whether the new changes still work.

---

# 12. GitHub Actions

**GitHub Actions** provides the automation machinery for GitHub.

An Action can automatically perform tasks when something happens in the repository.

For example:

```text
Push code
   ↓
GitHub Action starts
   ↓
Build project
   ↓
Run tests
   ↓
Report result
```

A workflow might run when:

* Code is pushed
* A Pull Request is opened
* A Pull Request is updated
* Code is merged into `main`

The important concept is:

> **GitHub Actions allows the development process to automatically respond to changes in the repository.**

---

# 13. CI in the DungeonForge Project

For DungeonForge, our CI pipeline might look like:

```text
Developer
    │
    ▼
Write Code
    │
    ▼
Commit
    │
    ▼
Push to GitHub
    │
    ▼
Pull to GitHub main branch 
    │
    ▼
GitHub Actions
    │
    ├── Build
    ├── Run Tests
    └── Check Code
          │
          ▼
       Pass / Fail
```

If the tests fail, the team gets immediate feedback.

If they pass, the change is much safer to merge.

This helps us maintain a working project throughout the semester.

---

# 14. Putting Everything Together

These technologies are not separate topics. They form a development system.

```text
                    AGILE
                      │
             Organizes the work
                      │
                      ▼
                User Stories
                      │
               Define features
                      │
                      ▼
               GitHub Projects
                      │
               Tracks the work
                      │
                      ▼
                     Git
                      │
              Tracks the code
                      │
                      ▼
                   GitHub
                      │
           Collaborates around Git
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
   Pull Requests            GitHub Actions
   Code Review                    │
                                  ▼
                    Continuous Integration
                                  │
                         Build + Test + Feedback
```

Each piece has a different job.

---

# 15. The DungeonForge Development Loop

Throughout the semester, the overall process will look something like:

```text
Choose a User Story
        ↓
Add it to the Project
        ↓
Create a Branch
        ↓
Implement the Feature
        ↓
Commit Changes
        ↓
Push to GitHub
        ↓
Open Pull Request
        ↓
CI Builds and Tests
        ↓
Review
        ↓
Merge
        ↓
Feature Complete
        ↓
Choose the Next Story
```

Then we repeat.

---

# 16. Final Takeaway

The important concepts to keep in mind are:

| Concept                    | Purpose                                                           |
| -------------------------- | ----------------------------------------------------------------- |
| **Agile**                  | Organizes development around incremental, iterative delivery      |
| **User Story**             | Describes valuable functionality from the user's perspective      |
| **Git**                    | Tracks and manages changes to the code                            |
| **GitHub**                 | Provides collaboration and development tools around Git           |
| **GitHub Projects**        | Organizes and tracks Agile work                                   |
| **Pull Request**           | Provides a controlled mechanism for reviewing and merging changes |
| **GitHub Actions**         | Automates development tasks                                       |
| **Continuous Integration** | Automatically builds and tests changes as they are integrated     |

The big picture is:

> **Agile tells us how to manage the work. Git tracks the work's code. GitHub provides the collaboration platform. GitHub Projects organizes the work. Pull Requests control how changes enter the shared codebase. GitHub Actions automates testing and other development tasks. CI gives the team rapid feedback that the project still works.**

For **DungeonForge**, these pieces will allow us to build a large OOP project incrementally while keeping the codebase organized, tested, and continuously working throughout the semester.
