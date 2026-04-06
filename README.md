# Tank War

A small 2D tank game built with **Java Swing** and **AWT**. The player moves a tank on an 800×600 battlefield with brick walls and a grid of enemy tanks. Rendering uses GIF/PNG sprites loaded from the local `assets` folder.

## Features

- **Player tank** — controlled with the keyboard; supports eight-way movement (including diagonals).
- **Enemy tanks** — twelve enemies arranged in a 3×4 formation.
- **Walls** — horizontal and vertical brick segments used as obstacles; tanks cannot pass through them or through each other.
- **Shooting**
  - **Space** — fires one missile in the current facing direction.
  - **A** — “super fire”: spawns missiles in all eight directions at once (player only).
- **Singleton game client** — `gameclient` holds the player, enemies, walls, and active missiles and repaints in a simple game loop (~20 FPS, 50 ms sleep).

## Requirements

- **JDK 8** or newer (project targets Java 8 in `pom.xml`)
- **Apache Maven** (to compile and run tests)

## Project layout

```
src/main/java/com/java/tankwar/
  gameclient.java   — entry point, window, key listener, game loop
  Tank.java         — movement, collision with walls/enemies, firing
  Missile.java      — projectile movement and drawing
  Wall.java         — brick walls
  Direction.java    — facing / sprite key (tank and missile GIFs)
  tools.java        — loads images from the assets directory

assets/
  images/           — tank, missile, brick, and UI images (GIF/PNG)
  audios/           — present in the repo (not wired in the current Java code)

src/test/java/      — JUnit 5 tests (e.g. tank image loading)
```

## Build and run

**Important:** Image paths are **relative to the process working directory**. Run the game from the **project root** so `assets/...` resolves correctly.

```bash
mvn compile
java -cp target/classes:. com.java.tankwar.gameclient
```

On Windows, use a semicolon in the classpath:

```cmd
mvn compile
java -cp target/classes;. com.java.tankwar.gameclient
```

### Tests

```bash
mvn test
```

## Controls

| Key | Action |
|-----|--------|
| ↑ ↓ ← → | Move (combinations give diagonal movement) |
| Space | Fire a single missile |
| A | Fire missiles in all eight directions |

## Technical notes

- **Window title** is set in Chinese (“坦克大戰”) in `gameclient`.
- **Window icon** is loaded from a **hard-coded absolute path** in `gameclient` (`.../assets/images/icon.png`). On another machine, change that path or the game may fail to set the icon (the rest of the game may still run).
- **`tools.getImage`** uses the path `assets/Images/` (capital `I`). The repository uses `assets/images/`. This works on **case-insensitive** file systems (default macOS) but can break on **Linux** or other case-sensitive setups unless the folder name matches the code or the path in `tools.java` is updated.
- **Combat logic** in this snapshot is minimal: missiles move and draw; there is no implemented logic here for hits, damage, or removing enemies/missiles from the lists.

## License

No license file is included in this repository; treat usage as unspecified until one is added.
