# AGENTS.md

Rules for all coding work in this repository. Follow them strictly for every language (C, C++, C#, Python, JavaScript, TypeScript, Rust, Go, etc.).

## 1. No Comments

- Never write code comments. No `//`, `/* */`, `#`, `///`, XML doc comments, docstrings, or TODO/FIXME notes.
- Do not add banner comments, section dividers, or commented-out code.
- When touching existing code, do not add comments. Remove only the comments belonging to code you are changing.
- Make the code self-explanatory through clear naming and small functions instead.
- Exceptions: comments that are functionally required by a tool (license headers demanded by the project, `#pragma`, shebangs, `# type:` directives, linter pragmas, encoding declarations).

## 2. Brace Style: K&R

- The opening brace goes on the same line as the statement, for control flow, functions, classes, structs, and namespaces.
- The closing brace goes on its own line, aligned with the start of the statement.
- `else`, `else if`, `catch`, and `finally` go on the same line as the preceding closing brace.
- Always use braces, even for single-statement bodies.
- Use 4 spaces for indentation. Never use tabs.

```csharp
public class Player {
    public void Update(float deltaTime) {
        if (health <= 0) {
            Respawn();
        } else if (isStunned) {
            return;
        } else {
            Move(deltaTime);
        }
    }
}
```

## 3. Spacing and Formatting

- Exactly one space around binary operators and assignments: `x = 0`, never `x =      0`.
- Never align code with extra spaces. No column-aligned assignments, declarations, comments, or table-like formatting.
- One space after commas and semicolons in `for` headers: `foo(a, b, c)`, `for (int i = 0; i < count; i++)`.
- One space between a keyword and its parenthesis: `if (x)`, `while (x)`, `for (...)`.
- No space between a function name and its parenthesis: `Update(deltaTime)`.
- No space inside parentheses or brackets: `foo(a)`, `items[i]`.
- No trailing whitespace. End every file with a single newline.
- At most one blank line in a row. Use blank lines to separate logical blocks, never at the start or end of a block.
- Keep lines under 120 characters. Break long expressions at operators or arguments with a standard indent.

## 4. Human-Like Code

- Write code the way an experienced developer would write it by hand.
- Use clear, common, descriptive names. No cryptic abbreviations, no invented jargon, no overly long names, no generic names like `Helper`, `Manager2`, `DoStuff`, `ProcessData`, `HandleThing`.
- Functions are named as verbs (`LoadConfig`, `calculate_damage`, `parse_header`). Classes, structs, and types are nouns (`Player`, `PhysicsWorld`, `FileReader`). Booleans read like questions (`isVisible`, `has_target`, `canJump`).
- Follow the naming conventions of the language:
  - C#: `PascalCase` for types, methods, and properties; `camelCase` for locals and parameters; `_camelCase` for private fields.
  - C / C++: match the existing codebase; default to `snake_case` for functions and variables and `PascalCase` for types.
  - Python: `snake_case` for functions and variables, `PascalCase` for classes, `UPPER_CASE` for constants.
- Keep functions short and focused on one job. Prefer early returns over deep nesting.
- Do not over-engineer. No unnecessary abstractions, interfaces, factories, or design patterns when a simple solution works.
- Do not write defensive boilerplate nobody needs, and do not add unused parameters, dead code, or placeholder methods.
- Use named constants instead of magic numbers.
- Prefer the standard library and idioms of the language over custom reimplementations.

## 5. Project Structure

The root contains only the entry point, project and build files, top-level configuration, and the `Scripts/` and `Tests/` folders. All other code lives in `Source/`.

### Root

- The entry file (main class) is always in the root: `Program.cs`, `main.cpp`, `main.py`, `main.rs`, `index.ts`, etc.
- Project and dependency files are always in the root: `.sln` / `.slnx`, `.csproj`, `CMakeLists.txt`, `requirements.txt`, `pyproject.toml`, `package.json`, `Cargo.toml`, `go.mod`, `Makefile`, etc.
- Also in the root: `README.md`, `AGENTS.md`, `.gitignore`, `.editorconfig`, and similar repository-level files.

### Source Folder

- All other source code goes in a `Source/` folder.
- Inside `Source/`, organize by feature, not by file type. Each feature gets its own folder.
- Do not create folders like `Classes/`, `Interfaces/`, `Enums/`, `Utils/`, or `Helpers/`.
- Name feature folders in `PascalCase` (or the convention of the language, e.g. `snake_case` for Python packages).
- Platform and API wrappers get their own folder named after the API, e.g. `Win32/`, `Vulkan/`, `OpenGL/`, `Sqlite/`.
- Split a feature folder into subfolders only when it grows large enough to need it.

### Scripts Folder

- Every project has a `Scripts/` folder in the root.
- It always contains at least one script to build the project and one script to run the tests: `build` and `test`.
- Use the script type that fits the platform: `.ps1` for Windows projects, `.sh` for Linux and macOS. Provide both when the project is cross-platform.
- Scripts must work when started from any directory and must exit with a non-zero code on failure.
- Always build and test through these scripts instead of typing the commands by hand.
- Other recurring tasks (run, clean, publish, format) also go into `Scripts/`.

### Tests Folder

- Tests live in a `Tests/` folder in the root, never mixed into `Source/`.
- The structure inside `Tests/` mirrors the feature folders in `Source/`: tests for `Source/Physics/` go in `Tests/Physics/`.
- For C# this is a separate test project referenced by the solution.

### Example (C#)

```
MyGame/
├── MyGame.sln
├── MyGame.csproj
├── Program.cs
├── README.md
├── AGENTS.md
├── Scripts/
│   ├── build.ps1
│   └── test.ps1
├── Tests/
│   └── Physics/
│       └── PhysicsWorldTests.cs
└── Source/
    ├── Physics/
    │   ├── PhysicsWorld.cs
    │   ├── RigidBody.cs
    │   └── Collision.cs
    ├── Rendering/
    │   ├── Renderer.cs
    │   └── Camera.cs
    ├── Input/
    │   └── KeyboardState.cs
    └── Win32/
        ├── NativeMethods.cs
        └── WindowHandle.cs
```

### Example (C++)

```
MyEngine/
├── CMakeLists.txt
├── main.cpp
├── README.md
├── AGENTS.md
├── Scripts/
│   ├── build.sh
│   └── test.sh
├── Tests/
│   └── Physics/
│       └── PhysicsWorldTests.cpp
└── Source/
    ├── Physics/
    │   ├── PhysicsWorld.cpp
    │   └── PhysicsWorld.h
    └── Win32/
        ├── Window.cpp
        └── Window.h
```

### Example (Python)

```
my_tool/
├── main.py
├── requirements.txt
├── README.md
├── AGENTS.md
├── Scripts/
│   ├── build.sh
│   └── test.sh
├── Tests/
│   └── parsing/
│       └── test_csv_reader.py
└── Source/
    ├── parsing/
    │   └── csv_reader.py
    └── networking/
        └── http_client.py
```

## 6. Testing

- Always test the code. No task is finished until the code has been built and the tests have passed.
- After every change, run `Scripts/build` and `Scripts/test` and read the output.
- Write tests for all new features and bug fixes. For a bug fix, add a test that fails without the fix.
- Test behavior and edge cases, not implementation details. Keep tests small, independent, and deterministic.
- Tests follow the same style rules as all other code: no comments, K&R braces, clean spacing, clear names. Name tests after what they check, e.g. `Update_WhenHealthIsZero_RespawnsPlayer`.
- If a test fails, fix the code. Never delete, skip, or weaken a test just to make it pass, unless the test itself is wrong.
- Never claim that something works or was tested without having run it. If the code cannot be built or tested in the current environment, say so clearly.

## 7. General Behavior

- Match the existing style of the file or project when editing, as long as it does not conflict with the rules above.
- Make the smallest change that solves the task. Do not reformat or refactor unrelated code.
- Do not add new files, dependencies, or folders unless the task requires them.
- Put every new file in the correct feature folder following the structure above.
- Keep one primary type per file, and name the file after that type.
