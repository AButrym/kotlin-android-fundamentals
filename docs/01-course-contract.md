# Kotlin for Android Fundamentals - Course Contract 
 
Status: Draft 
Version: 0.1.0 
Canonical repository path: docs/01-course-contract.md 
Course ID: kotlin-android-fundamentals 
 
## 1. Purpose of this document 
 
This document defines the stable contract for the Kotlin for Android Fundamentals course. It establishes the target audience, entry assumptions, learning outcomes, scope boundaries, pedagogical principles, capstone requirements, target application architecture, assessment strategy, and constraints that all later curriculum and generated artifacts must follow. 
 
This contract is the primary source of truth for: 
 
- the course knowledge map; 
- the concept dependency map; 
- detailed lesson specifications; 
- lesson-generation prompts; 
- exercises and embedded sandboxes; 
- Android Studio labs; 
- instructor materials; 
- generated HTML lessons; 
- course-level consistency reviews. 
 
When a generated artifact conflicts with this contract, the contract takes precedence unless the contract is explicitly revised. 
 
## 2. Course identity 
 
### 2.1 Title 
 
Kotlin for Android Fundamentals 
 
### 2.2 Format 
 
- 14 instructor-led lessons; 
- 120 minutes per lesson; 
- 28 total contact hours; 
- integrated Kotlin, programming fundamentals, Android Studio, and Jetpack Compose practice; 
- a continuous project trajectory ending in a playable grid-based Android game. 
 
### 2.3 Core course promise 
 
A learner with no prior programming experience can progress from executing a first Kotlin expression to building, explaining, and presenting a small state-driven Android application in Kotlin and Jetpack Compose. 
 
The course does not attempt to cover all Kotlin language features or production Android engineering. It teaches the minimum coherent set of concepts required to understand and build a small application with clear separation between game model, UI state, ViewModel, screen-level UI, and reusable Compose components. 
 
## 3. Target audience 
 
### 3.1 Primary audience 
 
The primary audience consists of learners who: 
 
- have no previous programming experience, or only very limited informal experience; 
- have not previously developed Android applications; 
- are unfamiliar with Kotlin; 
- can use a desktop operating system, install software, manage files, and use a web browser; 
- are prepared to practice between lessons; 
- are motivated by building visible, interactive applications rather than studying a language in isolation. 
 
### 3.2 Secondary audience 
 
The course may also support learners who: 
 
- know basic programming in another language but are new to Kotlin; 
- know basic Kotlin syntax but are new to Android and Jetpack Compose; 
- need a structured bridge from language fundamentals to a small Android application. 
 
The primary beginner path must not assume the secondary audience's prior knowledge. 
 
### 3.3 Audience implications 
 
Because the primary audience starts from zero, every required abstraction must be motivated by a concrete problem before or while it is introduced. 
 
The course must not rely on unexplained knowledge of: 
 
- algorithms; 
- object-oriented programming; 
- functional programming; 
- static types; 
- build systems; 
- version control; 
- reactive or declarative UI; 
- Android lifecycle; 
- software architecture patterns. 
 
## 4. Entry requirements 
 
### 4.1 Required prior knowledge 
 
No previous programming knowledge is required. 
 
Learners should be able to: 
 
- navigate files and folders; 
- install and launch applications; 
- type and edit text; 
- follow multi-step technical instructions; 
- read basic technical English used in identifiers, IDE commands, and documentation. 
 
### 4.2 Required environment 
 
Learners need: 
 
- a computer capable of running the supported Android Studio version; 
- a supported JDK as managed or recommended by Android Studio; 
- either an Android emulator or a physical Android device suitable for development; 
- a modern web browser for the HTML course and embedded exercises; 
- network access for installing required tools and dependencies; 
- Git and a Git hosting account when repository workflows are introduced. 
 
Exact supported versions must be defined in the environment setup artifact and verified against current official documentation before publication. 
 
### 4.3 Pre-course setup 
 
Installation and large SDK downloads should be completed before Lesson 1 whenever possible. Setup instructions may be provided as a separate onboarding activity so that the first lesson can focus on learning rather than waiting for installations. 
 
A fallback plan must exist for learners whose Android environment is temporarily unavailable. Kotlin sandbox activities and instructor demonstrations should allow a learner to continue with the conceptual portion of a lesson. 
 
## 5. Course-level learning outcomes 
 
By the end of the course, a successful learner should be able to perform the following tasks with limited guidance. 
 
### 5.1 Programming fundamentals 
 
The learner can: 
 
- explain at a beginner level what a program, expression, statement, value, variable, type, function, condition, loop, object, and collection are; 
- trace small programs step by step; 
- decompose a small problem into functions and data; 
- use conditions and repetition to express simple algorithms; 
- distinguish changing state from derived values; 
- read common compiler messages and runtime stack traces at a basic level; 
- debug small programs using output, IDE diagnostics, and a debugger where appropriate. 
 
### 5.2 Kotlin fundamentals 
 
The learner can: 
 
- declare immutable and mutable variables using `val` and `var`; 
- use fundamental types such as `Int`, `Double`, `Boolean`, and `String`; 
- build expressions with arithmetic, comparison, Boolean, and string operations; 
- use `if`, `when`, ranges, `for`, and `while`; 
- declare and call functions with parameters and return values; 
- understand `Unit` at a practical beginner level; 
- define simple classes, properties, methods, constructors, and data classes; 
- work with lists and use sets or maps when the problem naturally requires them; 
- use nullable types, safe calls, the Elvis operator, and basic smart casts; 
- use enums and sealed hierarchies to model a small finite domain; 
- use lambdas and common collection transformations such as `map`, `filter`, `find`, `any`, `all`, and `count`; 
- use extension functions where they improve domain readability; 
- prefer immutable data and `copy` when modeling UI state; 
- read and write small idiomatic Kotlin programs without relying on advanced language features. 
 
### 5.3 Android Studio and development workflow 
 
The learner can: 
 
- open, build, run, and make small changes to an Android Studio project; 
- run an application on an emulator or physical device; 
- identify the main project areas relevant to a beginner Compose application; 
- use IDE navigation, code completion, formatting, and basic refactoring; 
- interpret common build, compilation, and runtime failures at a basic level; 
- understand that Gradle configures the build and dependencies without needing to master Gradle internals; 
- use a basic Git workflow to preserve and share progress. 
 
### 5.4 Jetpack Compose fundamentals 
 
The learner can: 
 
- explain declarative UI as describing UI from current state; 
- create and call composable functions; 
- use Material components such as text, buttons, cards, and common input controls; 
- arrange UI using `Row`, `Column`, `Box`, and relevant modifiers; 
- render repeated content using collections and suitable lazy layouts or grids; 
- distinguish parameters from local state; 
- use state to drive visible UI changes; 
- apply state hoisting so data flows down and events flow up; 
- create reusable stateless UI components where practical; 
- apply a basic Material theme and produce a usable beginner-level layout; 
- recognize recomposition as Compose re-evaluating affected UI when observed state changes, without requiring knowledge of runtime internals. 
 
### 5.5 Application architecture 
 
The learner can: 
 
- distinguish game or application rules from UI rendering; 
- identify domain model, UI state, ViewModel, screen, and reusable component responsibilities; 
- explain unidirectional data flow in the final application; 
- trace a user action from a composable callback to the ViewModel and then to updated UI state; 
- explain why a Screen should not contain all game rules; 
- explain why reusable components should receive required data and callbacks instead of owning unrelated application state; 
- place a small application feature into a simple package and file structure based on responsibilities; 
- make a small architecture-preserving change to the final project. 
 
### 5.6 Final project 
 
The learner can: 
 
- implement a playable grid-based game using Kotlin and Jetpack Compose; 
- represent the board, players, turns, and game status in program data; 
- validate or process moves according to the selected game's rules; 
- render the board and status from current state; 
- handle user actions through explicit events or ViewModel functions; 
- restart or reset a game; 
- explain the main control and data flow; 
- demonstrate the application and discuss at least one design decision. 
 
## 6. Course design principles 
 
### 6.1 One integrated course, not two parallel subjects 
 
Kotlin and Android must not be taught as unrelated tracks. New Kotlin concepts should be connected to visible Android behavior as soon as practical, and Android problems should motivate later Kotlin concepts. 
 
The preferred learning loop is: 
 
1. Present a concrete problem or desired behavior. 
2. Build intuition with a small example. 
3. Introduce the minimum new programming or Kotlin concept. 
4. Practice the concept in an embedded sandbox. 
5. Apply the concept in Android Studio or the continuous project. 
6. Reflect on the resulting behavior and code structure. 
 
### 6.2 Gradual shift in lesson emphasis 
 
Every lesson contains both a Kotlin/programming component and an Android/Compose component. 
 
The approximate balance should shift across the course: 
 
- early lessons: about 90 minutes Kotlin/programming and 30 minutes Android/Compose; 
- middle lessons: about 60 minutes Kotlin/programming and 60 minutes Android/Compose; 
- late lessons: about 30 minutes Kotlin/programming and 90 minutes Android/Compose/project work. 
 
These are planning targets rather than rigid lecture blocks. A lesson should interleave concepts and application when that produces a clearer learning sequence. 
 
### 6.3 Spiral progression 
 
Important concepts should evolve through explicit stages: 
 
- previewed: encountered informally without requiring mastery; 
- introduced: explained and practiced directly; 
- reinforced: reused in varied guided contexts; 
- applied: used with less scaffolding in a meaningful task; 
- assumed: treated as established prerequisite knowledge. 
 
A concept may be previewed before formal introduction, but generated materials must not silently depend on knowledge that has not yet been introduced. 
 
### 6.4 Concrete before abstract 
 
A concrete need should precede a major abstraction whenever possible. 
 
Examples: 
 
- nullable data is motivated by an empty cell or absent winner; 
- a data class is motivated by grouping related cell or player data; 
- a collection is motivated by representing a board; 
- a sealed hierarchy is motivated by finite game states; 
- state hoisting is motivated by duplicated or misplaced UI state; 
- ViewModel is motivated by excessive logic and state inside a screen; 
- reusable components are motivated by repeated UI and clear responsibilities. 
 
### 6.5 Visible progress in every lesson 
 
Every lesson should end with a visible result such as: 
 
- changed application behavior; 
- a new reusable component; 
- an improved board model; 
- a completed rule; 
- a clearer architecture; 
- a working project milestone. 
 
The learner should not spend many consecutive lessons studying language features without applying them to an observable outcome. 
 
### 6.6 Minimum sufficient theory 
 
The course teaches theory required to build correct mental models and solve course tasks. It avoids comprehensive language or framework surveys. 
 
Advanced details can appear in clearly separated optional sections, but optional material must not become a hidden prerequisite for the core path. 
 
### 6.7 Productive struggle with bounded difficulty 
 
Exercises should require learners to predict, modify, complete, debug, and create code. Tasks should be challenging enough to require reasoning but constrained enough to be completed with the concepts currently available. 
 
Solutions should not depend on tricks, features not yet introduced, or extensive boilerplate unrelated to the lesson outcome. 
 
### 6.8 Explainable architecture 
 
The final architecture should be simple enough for a beginner to explain. Architectural separation is valuable only when each boundary solves a visible problem. 
 
The course must avoid ceremonial layers introduced only because they are common in larger production applications. 
 
## 7. Delivery and artifact principles 
 
### 7.1 Primary learning artifact 
 
The primary learner-facing artifact is a continuous HTML course with embedded interactive or editable Kotlin exercises where technically feasible. 
 
Each lesson should include: 
 
- motivation and learning goals; 
- prerequisite recall; 
- conceptual explanations; 
- worked examples; 
- prediction tasks; 
- code modification tasks; 
- embedded Kotlin sandbox activities; 
- an Android Studio lab; 
- a continuous-project increment; 
- common mistakes and debugging guidance; 
- a short knowledge check; 
- an optional advanced challenge; 
- a concise lesson summary. 
 
### 7.2 Separation of canonical specifications and generated output 
 
Canonical sources include: 
 
- this course contract; 
- the course knowledge map; 
- the concept dependency map; 
- detailed lesson specifications; 
- artifact and template specifications; 
- approved architecture and style decisions. 
 
Generated HTML is a delivery artifact, not a canonical curriculum specification. 
 
### 7.3 Source policy 
 
Technical claims must prefer current official sources, especially: 
 
- Kotlin documentation; 
- Android Developers documentation; 
- Jetpack Compose documentation; 
- Material Design documentation; 
- Android Studio and Gradle documentation when relevant. 
 
Secondary sources, including TypeAlias and selected structured learning platforms, may be used for intuition, illustrations, alternative explanations, and exercise inspiration. Secondary sources must not override current official behavior or recommendations. 
 
Generated material must not reproduce copyrighted source text. It should explain concepts in original language and cite or link to sources where useful. 
 
### 7.4 Language policy 
 
Canonical repository artifacts and code identifiers may use English to support tooling, external agents, and technical consistency. 
 
Learner-facing explanations may be generated in Ukrainian, English, or another explicitly selected delivery language. Technical names, API names, identifiers, and standard product names should remain in their conventional form. 
 
The selected language must be consistent within each delivered lesson unless a bilingual artifact is explicitly requested. 
 
## 8. Scope 
 
### 8.1 Core programming scope 
 
The required programming scope includes: 
 
- program execution and source code; 
- expressions and values; 
- variables and types; 
- arithmetic and Boolean logic; 
- strings and basic formatting; 
- conditions; 
- ranges and loops; 
- functions and decomposition; 
- basic debugging; 
- simple classes and objects; 
- data classes; 
- collections; 
- nullability; 
- lambdas and selected higher-order collection operations; 
- finite-state modeling with enums and sealed types; 
- immutable updates; 
- basic event and state reasoning. 
 
### 8.2 Core Kotlin scope 
 
Required Kotlin features include: 
 
- `fun`; 
- `val` and `var`; 
- type inference and explicit type declarations; 
- fundamental types; 
- string templates; 
- `if` and `when` expressions; 
- ranges; 
- `for` and `while` loops; 
- function parameters and return values; 
- named and default arguments where useful; 
- classes, constructors, properties, and methods; 
- data classes and `copy`; 
- read-only and mutable collections at a beginner level; 
- nullable types, `?.`, `?:`, and practical smart casts; 
- enums; 
- sealed interfaces or sealed classes; 
- lambdas and function types at a practical level; 
- common collection operations; 
- extension functions; 
- basic use of scope functions only where they clearly improve code. 
 
### 8.3 Core Android and Compose scope 
 
Required Android and Compose topics include: 
 
- Android Studio project workflow; 
- running on emulator or device; 
- a beginner-level overview of project structure; 
- composable functions; 
- Material components; 
- layouts and modifiers; 
- repeated and lazy UI; 
- user input and callbacks; 
- state-driven UI; 
- `remember` and local state for appropriate early examples; 
- state hoisting; 
- immutable UI state; 
- ViewModel as screen-level state holder; 
- simple unidirectional data flow; 
- reusable components; 
- basic theming and Material Design; 
- simple screen switching or navigation only to the extent required by the selected final project. 
 
### 8.4 Core development workflow scope 
 
The course includes a minimal practical workflow for: 
 
- building and running; 
- reading diagnostics; 
- debugging small failures; 
- formatting and basic refactoring; 
- Git commits and basic repository use; 
- understanding dependencies at a user level. 
 
Git and Gradle are integrated tools, not standalone theory-heavy lesson goals. 
 
## 9. Non-goals and excluded core topics 
 
The following topics are outside the required core curriculum unless a later approved decision changes the contract. 
 
### 9.1 Advanced Kotlin 
 
- variance and advanced generic declarations; 
- custom iterators and iterable implementations; 
- operator overloading; 
- infix function design; 
- delegated properties as a language topic; 
- custom delegates; 
- detailed lazy initialization semantics; 
- reflection; 
- annotations beyond those required for Compose; 
- DSL construction; 
- contracts; 
- advanced inline and reified generics; 
- multiplatform development; 
- coroutines and Flow as required learning outcomes. 
 
Existing framework APIs may expose an advanced concept, but generated materials must not require learners to master the underlying advanced Kotlin topic. 
 
### 9.2 Production Android infrastructure 
 
- dependency injection frameworks such as Hilt or Koin; 
- Room or other database frameworks; 
- network clients and remote APIs; 
- repository layers introduced without a real data source; 
- use-case layers introduced only for architectural ceremony; 
- modularization; 
- build-logic engineering; 
- advanced Gradle configuration; 
- production navigation architecture; 
- background work; 
- services, broadcast receivers, and content providers; 
- permissions beyond what a chosen optional project genuinely requires; 
- release signing, store publication, analytics, and monetization; 
- comprehensive lifecycle internals; 
- performance profiling and optimization; 
- comprehensive automated UI testing. 
 
### 9.3 Computer science depth outside the course goal 
 
- formal algorithm analysis beyond intuitive cost awareness; 
- recursion as a required problem-solving technique; 
- complex data structures; 
- graph algorithms; 
- comprehensive design-pattern catalogs; 
- formal software architecture taxonomies. 
 
### 9.4 Scope-control rule 
 
An excluded topic may appear in an optional deep-dive section only if: 
 
- it is clearly labeled optional; 
- the core exercise does not depend on it; 
- it does not displace required practice; 
- it does not introduce conflicting architecture; 
- it is appropriate for the learner's current conceptual level. 
 
## 10. Continuous project and capstone 
 
### 10.1 Project trajectory 
 
The course uses small applications and a gradually evolving grid/game domain. Early applications may be independent micro-projects, but concepts and components should progressively converge toward the final game. 
 
The planned trajectory should approximately include: 
 
- a counter or clicker; 
- conditional interactive UI; 
- a repeated or grid-like layout; 
- a reusable cell component; 
- a cell and board model; 
- player and turn state; 
- game status; 
- move processing and win or completion rules; 
- state hoisting and unidirectional flow; 
- ViewModel extraction; 
- Material styling and screen composition; 
- an independently completed final game. 
 
### 10.2 Minimum capstone level 
 
The minimum successful capstone is a complete small grid-based game with complexity comparable to Tic-Tac-Toe. 
 
The application must support: 
 
- a visible board; 
- legal user interaction with cells or tiles; 
- explicit representation of current state; 
- at least two meaningful game statuses, such as playing and finished; 
- rule evaluation; 
- visible feedback about progress or result; 
- reset or new game behavior; 
- separation between game logic and UI rendering; 
- a ViewModel-managed screen state; 
- reusable Compose components. 
 
### 10.3 Standard capstone options 
 
Suitable standard projects include: 
 
- Tic-Tac-Toe; 
- Fifteen Puzzle; 
- Connect Four; 
- Memory matching game; 
- another instructor-approved game with comparable scope. 
 
### 10.4 Advanced capstone options 
 
Advanced learners may implement: 
 
- Reversi; 
- simplified checkers; 
- hot-seat Battleship; 
- another instructor-approved turn-based grid game. 
 
An advanced project may use an intermediate handover screen when two learners or players take turns on one device and hidden information must not be shown to the next player. 
 
Advanced scope must not change the minimum learning outcomes for the whole cohort. 
 
### 10.5 Capstone acceptance criteria 
 
A capstone is complete when: 
 
- the project builds and launches; 
- the primary gameplay loop works; 
- invalid actions are prevented or handled consistently; 
- game state is represented in Kotlin data rather than inferred only from rendered UI; 
- game logic is not embedded entirely inside cell composables; 
- UI is derived from current state; 
- user actions reach the state holder through explicit callbacks or functions; 
- the game can restart; 
- the code structure is understandable at beginner level; 
- the learner can explain the data and event flow; 
- known limitations are documented. 
 
Visual polish is desirable but cannot compensate for missing state, rules, or architecture. 
 
## 11. Target architecture 
 
### 11.1 Architectural goal 
 
The final project should make the following responsibilities visible: 
 
- model: represents cells, players, board data, game status, and domain concepts; 
- game logic: validates and applies moves and derives game results; 
- UI state: contains the data required to render the current screen; 
- ViewModel: owns or coordinates screen state and responds to user actions; 
- Screen: connects ViewModel state and actions to UI structure; 
- components: render reusable parts such as board, cell, status, controls, and handover UI; 
- Material theme: provides consistent visual styling. 
 
### 11.2 Target data and event flow 
 
The target flow is: 
 
1. ViewModel exposes current UI state. 
2. Screen observes or receives that state. 
3. Screen passes required data and callbacks to child components. 
4. A component emits a user event through a callback. 
5. Screen forwards the event to the ViewModel or an appropriate action handler. 
6. ViewModel validates and applies the action using game logic. 
7. ViewModel creates updated immutable state. 
8. Compose renders the UI from the new state. 
 
Materials should describe this as state flowing down and events flowing up. 
 
### 11.3 Minimum architecture 
 
The final project should use a structure equivalent to: 
 
    feature/game/ 
      model/ 
        Cell.kt 
        Player.kt 
        GameStatus.kt 
      GameRules.kt 
      GameUiState.kt 
      GameViewModel.kt 
      GameScreen.kt 
      components/ 
        GameBoard.kt 
        GameCell.kt 
        GameStatusPanel.kt 
 
The exact package names may differ. 
 
For a very small beginner project, some model files may be combined. Conceptual responsibilities must nevertheless remain identifiable. 
 
### 11.4 Architecture constraints 
 
Required: 
 
- game rules remain testable or callable independently from individual UI components; 
- the Screen does not manually store duplicate copies of ViewModel-owned state; 
- components receive only the data and callbacks they need; 
- state changes are explicit and predictable; 
- immutable state updates are preferred; 
- architecture is introduced incrementally after learners experience the problems it solves. 
 
Not required: 
 
- repository pattern; 
- use-case classes; 
- dependency injection framework; 
- database layer; 
- network layer; 
- multi-module architecture; 
- strict adherence to a named enterprise architecture. 
 
### 11.5 Architecture teaching constraint 
 
The final architecture must not be shown as unexplained boilerplate at the beginning of the course. The course should evolve toward it through refactoring: 
 
- local UI behavior; 
- modeled data; 
- lifted state; 
- state-driven screen; 
- extracted game logic; 
- ViewModel-managed state; 
- reusable components. 
 
## 12. Lesson contract 
 
Every lesson specification must define: 
 
- lesson identifier and title; 
- role in the overall course; 
- entry knowledge; 
- measurable exit capabilities; 
- programming concepts; 
- Kotlin concepts; 
- Android and Compose concepts; 
- architecture concepts, if any; 
- required vocabulary; 
- mental models; 
- common misconceptions; 
- recommended narrative sequence; 
- guided examples; 
- sandbox activities; 
- Android Studio lab; 
- continuous-project increment; 
- formative assessment; 
- homework or independent practice; 
- advanced track; 
- explicit out-of-scope items; 
- dependencies on previous and subsequent lessons; 
- source requirements. 
 
### 12.1 Lesson timing model 
 
A 120-minute lesson should normally include: 
 
- retrieval or activation of prior knowledge; 
- explanation and guided practice; 
- short checks for understanding; 
- hands-on Kotlin or Compose work; 
- an Android Studio application increment; 
- closure and next-step guidance. 
 
Exact minute-by-minute schedules belong in lesson specifications, not in this contract. 
 
### 12.2 Core and advanced paths 
 
Every lesson may include: 
 
- Core: required for all learners and required by later lessons; 
- Practice: additional repetition at the same conceptual level; 
- Challenge: optional task for faster or more experienced learners; 
- Deep Dive: optional explanation not required for course progression. 
 
Generated materials must clearly distinguish these paths. 
 
## 13. Assessment strategy 
 
### 13.1 Assessment purpose 
 
Assessment should verify conceptual understanding and practical ability rather than memorization of syntax. 
 
### 13.2 Formative assessment 
 
Each lesson should use several lightweight checks such as: 
 
- predict the output; 
- identify a bug; 
- complete a missing expression or function; 
- explain state before and after an action; 
- modify behavior under a constraint; 
- classify responsibility as model, state holder, screen, or component; 
- demonstrate a project increment; 
- answer a short reflective question. 
 
### 13.3 Cumulative checkpoints 
 
The curriculum should include cumulative checkpoints around these stages: 
 
- programming foundations; 
- data modeling and collections; 
- state-driven Compose; 
- ViewModel and architecture; 
- final project readiness. 
 
A checkpoint must assess both retained knowledge and integration, not only the most recent lesson. 
 
### 13.4 Capstone assessment dimensions 
 
The final project should be reviewed across: 
 
- correctness of the gameplay loop; 
- quality of state representation; 
- separation of responsibilities; 
- appropriate use of Kotlin fundamentals; 
- appropriate use of Compose state and components; 
- basic usability and visual clarity; 
- ability to explain the implementation; 
- ability to make or describe a small change. 
 
### 13.5 Definition of successful completion 
 
A learner successfully completes the course when the learner: 
 
- demonstrates the required core outcomes at a basic independent level; 
- submits or presents a functioning minimum capstone; 
- can explain the main architecture and state flow; 
- can complete a small modification or debugging task without replacing the project with a copied solution. 
 
## 14. Exercise and sandbox constraints 
 
### 14.1 Exercise progression 
 
Exercises should move through: 
 
1. Observe. 
2. Predict. 
3. Run. 
4. Explain. 
5. Modify. 
6. Complete. 
7. Debug. 
8. Create. 
 
Not every lesson must use every stage, but the overall course should move from high scaffolding to greater independence. 
 
### 14.2 Embedded sandbox role 
 
Embedded sandboxes are primarily for: 
 
- pure Kotlin expressions; 
- control flow; 
- functions; 
- data modeling; 
- collection operations; 
- rule logic; 
- state transformations that do not require the Android runtime. 
 
Android-specific behavior should be practiced in Android Studio unless an approved browser environment reliably supports the required Compose experience. 
 
### 14.3 Exercise validity 
 
Every required exercise must: 
 
- use only introduced or explicitly previewed concepts; 
- have a clear completion condition; 
- avoid accidental complexity; 
- include useful feedback or a verification path; 
- support the stated lesson outcome; 
- have an instructor solution or reference approach; 
- avoid depending on hidden advanced language features. 
 
## 15. Instructor and generator constraints 
 
### 15.1 Do not front-load terminology 
 
A generated lesson must not introduce many formal terms before learners encounter the problem those terms describe. 
 
### 15.2 Do not silently use future concepts 
 
If an example requires a future concept, the artifact must do one of the following: 
 
- replace the example with an earlier-compatible version; 
- label the construct as a preview and explain only what is necessary; 
- provide the construct as fixed boilerplate that learners are not expected to understand yet; 
- move the example to a later lesson. 
 
### 15.3 Avoid fake simplicity 
 
Materials must not conceal essential state or logic in unexplained helper code merely to make the visible example shorter. 
 
Boilerplate may be hidden or provided when it is unrelated to the lesson, but the artifact must clearly identify what learners are and are not expected to understand. 
 
### 15.4 Preserve conceptual consistency 
 
The same concepts should use stable terminology across lessons. Changes in model or architecture should be presented as explicit refactorings and should explain the problem solved by the new structure. 
 
### 15.5 Prefer small realistic examples 
 
Examples should be small enough to understand but realistic enough to transfer to the final project. Arbitrary examples such as unrelated animal inheritance hierarchies should be avoided when the same concept can be explained through the course domain. 
 
## 16. Accessibility and inclusion 
 
Course artifacts should: 
 
- avoid relying on color alone to communicate state; 
- use readable contrast and typography; 
- provide text alternatives for meaningful visuals; 
- support keyboard interaction where the delivery platform permits; 
- use clear labels and instructions; 
- provide copyable code; 
- distinguish required from optional work; 
- avoid assuming fast typing or prior command-line familiarity; 
- include recovery guidance after common setup and code errors. 
 
Game examples should remain understandable to learners unfamiliar with a particular game. Each selected game must include a concise rules explanation or link to approved rules. 
 
## 17. Quality gates for generated course artifacts 
 
A lesson artifact is not ready for publication until it passes the following checks. 
 
### 17.1 Curriculum alignment 
 
- Outcomes match the course map. 
- Prerequisites match the dependency map. 
- Required concepts are covered. 
- Future concepts are not required prematurely. 
- The project increment supports the capstone trajectory. 
 
### 17.2 Technical correctness 
 
- Kotlin examples compile in the declared environment. 
- Android examples build with the declared project baseline. 
- APIs and recommendations are current at the publication date. 
- Explanations match observed behavior. 
- Solutions satisfy exercise requirements. 
 
### 17.3 Pedagogical quality 
 
- The lesson begins from a meaningful problem. 
- Explanations form a coherent progression. 
- Examples expose rather than hide the target concept. 
- Practice is sufficient for the importance of the concept. 
- Misconceptions and likely errors are addressed. 
- The required workload is realistic for 120 minutes. 
 
### 17.4 Artifact quality 
 
- HTML structure follows the approved template. 
- Code is copyable and readable. 
- Interactive controls work. 
- Navigation and headings are coherent. 
- Accessibility requirements are met. 
- Required sources are cited or linked. 
- Core, challenge, and deep-dive sections are visibly distinct. 
 
## 18. Change control 
 
### 18.1 Stable identifiers 
 
Course, lesson, concept, outcome, and artifact identifiers should remain stable after publication. Titles and descriptions may evolve, but identifier changes require explicit migration. 
 
### 18.2 Decision records 
 
A decision record should be created when a change: 
 
- affects several lessons; 
- changes concept ordering; 
- changes capstone requirements; 
- changes architecture boundaries; 
- adds or removes a core topic; 
- contradicts a common course-design expectation; 
- changes generated artifact contracts. 
 
### 18.3 Versioning 
 
This contract should use semantic-style document versions: 
 
- patch: clarification without changing learner obligations; 
- minor: compatible addition or refinement; 
- major: change to required outcomes, scope, course duration, capstone, or architecture. 
 
### 18.4 Reconciliation rule 
 
When canonical artifacts conflict, the conflict must be resolved explicitly rather than left for a generation agent to interpret. 
 
The intended precedence is: 
 
1. approved course contract; 
2. approved decision records that amend the contract; 
3. course knowledge map; 
4. concept dependency map; 
5. detailed lesson specifications; 
6. artifact specifications and templates; 
7. generated content. 
 
If a lower-level artifact exposes a flaw in a higher-level artifact, revise the higher-level artifact first and then regenerate or reconcile dependent artifacts. 
 
## 19. Open decisions 
 
The following decisions remain outside this initial contract and should be resolved in later artifacts or decision records: 
 
- exact supported Android Studio, Kotlin, Compose, Gradle, and JDK versions; 
- final repository package naming convention; 
- exact browser sandbox implementation; 
- whether navigation uses a navigation library or minimal explicit screen switching; 
- whether the core capstone is chosen by each learner or standardized for the cohort; 
- exact homework volume; 
- exact grading weights, if formal grading is required; 
- localization and bilingual-content strategy; 
- automated test coverage expectations; 
- final hosting and deployment workflow for the HTML course. 
 
These decisions must not silently expand the course beyond 28 contact hours. 
 
## 20. Contract summary 
 
The course is designed for true beginners and uses Android development as the motivating context for learning programming and Kotlin. 
 
The required progression is: 
 
Programming foundations -> Kotlin modeling -> Compose UI -> state-driven design -> ViewModel and simple architecture -> playable grid-based game. 
 
The course prioritizes: 
 
- coherent mental models; 
- immediate application; 
- visible progress; 
- repeated practice; 
- explainable code; 
- minimum sufficient architecture; 
- a working capstone. 
 
The course deliberately excludes advanced Kotlin and production Android infrastructure that does not directly support the beginner learning outcome. 
 
All future course artifacts must preserve these priorities and constraints. 
 