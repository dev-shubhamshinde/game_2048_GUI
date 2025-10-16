# 2048 Game (Java Swing)

---

## Summary

This is a Java implementation of the popular **2048** puzzle game using **Swing** (desktop GUI). The project follows an MVC-like structure with separate `model`, `view`, and `controller` packages.

---

## What I produced for you

* A detailed **README** (this document) containing installation, running, packaging, gameplay instructions and implementation notes.

---

## Prerequisites

* Java Development Kit (JDK) 11 or later (JRE 11+ is enough to run the compiled classes/JAR).
* (Optional) An IDE such as IntelliJ IDEA or Eclipse for development.

---

## Project structure (key files)

```
2048_Game/
├─ src/
│  ├─ Main.java                 # entry point
│  ├─ controller/GameOf2048Controller.java
│  ├─ model/Board.java
│  ├─ model/GameOf2048.java
│  └─ view/GameOf2048View.java
├─ out/production/2048_Game/    # compiled .class files (provided)
└─ .idea/                       # IntelliJ project files
```

---

## Running locally (quick, using provided compiled classes)

If you have a Java runtime installed, you can run the included compiled classes without recompiling:

1. Open a terminal and `cd` to the project root (the folder that contains `out/production/2048_Game`). Example:

```bash
cd path/to/2048_Game
java -cp out/production/2048_Game Main
```

This runs the `Main` class (the project uses the default package). If you get errors about missing classes, recompile as shown below.

---

## Compiling & running from source

If you want to compile from source (recommended if you modified code):

### Linux / macOS / WSL

```bash
cd path/to/2048_Game
# compile
javac -d out/production/2048_Game src/*.java src/controller/*.java src/model/*.java src/view/*.java
# run
java -cp out/production/2048_Game Main
```

### Windows (PowerShell)

```powershell
cd path\to\2048_Game
javac -d out\production\2048_Game src\*.java src\controller\*.java src\model\*.java src\view\*.java
java -cp out\production\2048_Game Main
```

Notes:

* `-d out/production/2048_Game` tells `javac` to output `.class` files into the same layout that the project currently uses.
* If `javac` is not found, install the JDK and ensure `javac` is on your PATH.

---

## Creating a runnable JAR

After successful compilation, create a jar with `Main` as entry point:

```bash
# create jar (from project root)
jar cfe 2048-game.jar Main -C out/production/2048_Game .
# run the jar
java -jar 2048-game.jar
```

If `jar` is not found, it is bundled with the JDK.

---

## Packaging into a native installer (optional)

* Use `jpackage` (bundled with recent JDKs) to create platform-specific installers / executables.
* Or use third-party wrappers (Launch4j + Inno Setup for Windows, AppBundler for macOS).

Example `jpackage` command (Linux/Mac/Windows - modify paths):

```bash
jpackage --input out/production/2048_Game --main-jar 2048-game.jar --name 2048Game --main-class Main --type app-image
```

## Gameplay instructions

* Objective: create a tile with value **2048**.
* Use the **arrow keys** to slide tiles in the chosen direction.
* When two tiles with the same number collide, they merge into a single tile with the sum of their values (this increases your score by that amount).
* After each valid move a new tile (2 or 4) appears at a random empty cell.
* Game ends when there are no valid moves left.

Controls implemented:

* Arrow keys (Up, Down, Left, Right) are handled. Comments mention `W/A/S/D` but only arrow keys are wired.

---

## Implementation details (high-level)

* **Architecture:** Simple MVC pattern

  * `model.Board` - holds the 4x4 integer grid and provides low-level utilities: adding random tiles, checking available moves, shifting/merging logic. Constants: `SIZE = 4`, `WINNING_TILE = 2048`, `EMPTY_TILE = 0`.
  * `model.GameOf2048` - game rules and higher-level move functions (moveLeft/moveRight/moveUp/moveDown); maintains the `score` and detects win/loss.
  * `view.GameOf2048View` - Swing `JFrame`-based GUI. A `GridLayout` of custom `GameOf2048Tile` (extends `JLabel`) displays numbers and colored tiles, and a score label.
  * `controller.GameOf2048Controller` - forwards keyboard input from the view to the model, updates the view, and checks for game end.

* **Tile generation:** Board uses `java.util.Random` to add `2` or `4` to an empty cell.

* **Controls & input:** KeyListener in the view forwards arrow key presses to controller.

---

## Known limitations & suggestions

* The GUI uses Swing (desktop-only), so a browser deployment requires a rewrite.
* `W/A/S/D` keys are mentioned in comments but not hooked; adding them is a small change in the KeyListener.
* No unit tests included — consider extracting pure game logic (model) and adding JUnit tests.
* Accessibility: consider large fonts / keyboard-only navigation improvements.
* UX: add a restart button, undo / persistent high scores, animations.
