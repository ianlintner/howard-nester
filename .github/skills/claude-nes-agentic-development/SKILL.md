---
name: Claude NES Agentic Development
description: Use this skill for LLM-driven NES game development in 6502 assembly with iterative planning, coding, testing, and debugging.
---

# Claude NES Agentic Development

## Purpose
Guide LLM agents (such as Claude) to build, refactor, test, and debug NES games in 6502 assembly and C. This skill integrates curated wiki projects, tooling references, core memory maps, architectural guardrails, and modular prompt templates to enable reliable, iterative development under strict NES hardware constraints.

## Core Reference Index
- **NESdev Wiki Home**: [https://www.nesdev.org/wiki/Nesdev_Wiki](https://www.nesdev.org/wiki/Nesdev_Wiki)
- **Programming Guide**: [https://www.nesdev.org/wiki/Programming_guide](https://www.nesdev.org/wiki/Programming_guide) — The foundational assembly/hardware reference.
- **Projects & Homebrew Directory**: [https://www.nesdev.org/wiki/Projects](https://www.nesdev.org/wiki/Projects) — Curated collection of community games, engines, and tools.
- **Development Tools Directory**: [https://www.nesdev.org/wiki/Tools](https://www.nesdev.org/wiki/Tools) — Comprehensive catalog of retro and modern tools.
- **6502 CPU Instruction Set**: [https://www.nesdev.org/wiki/6502_instructions](https://www.nesdev.org/wiki/6502_instructions) — Opcode matrix, addressing modes, and cycle counts.
- **PPU Registers Reference**: [https://www.nesdev.org/wiki/PPU_registers](https://www.nesdev.org/wiki/PPU_registers) — Ports $2000–$2007, registers, scroll offsets, and timings.
- **APU Audio Reference**: [https://www.nesdev.org/wiki/APU](https://www.nesdev.org/wiki/APU) — Sound channels, sweep units, and envelopes.

---

## Comprehensive NES Tooling Directory

Based on the official NESdev Wiki lists, use the following tools depending on the project's language stack, complexity, and assets:

### 1. Assemblers, Linkers & Compilers
*   **cc65 / ca65 / ld65**: 
    *   *Type*: Standard modern toolchain (C compiler, macro assembler, linker).
    *   *Strengths*: Industrial-grade modular design, robust config scripts (`.cfg`) allowing full custom memory mapping, powerful macro system, excellent integration of assembly with C.
    *   *Best for*: Large, multi-banked modular games, hybrid C/Assembly development, and production-grade software.
*   **asm6 / asm6f**:
    *   *Type*: Lightweight, single-pass assembler (asm6f adds modern extensions like Famicom Disk System support and enhanced output types).
    *   *Strengths*: Zero setup required, incredibly simple syntax, outputs ROMs directly without needing complex linker configuration files.
    *   *Best for*: Smaller homebrew games, game jams, single-file demos, and platforms like NESmaker.
*   **NESASM / NESASM3**:
    *   *Type*: Legacy 6502 assembler.
    *   *Strengths*: Used in classic tutorials (like Nerdy Nights), very simple to understand for beginners.
    *   *Limitations*: Strict 8KB banking constraints, non-standard syntax for certain addressing modes, and lacks the modular linkers of cc65.
*   **NESFab**:
    *   *Type*: A modern high-level programming language designed specifically for the NES.
    *   *Strengths*: Features static typing, safety features, high-performance compilation, and native multi-banking/mapper support out of the box.
    *   *Best for*: Rapid prototyping and developers seeking a modern alternative to pure assembly or low-performance C.

### 2. Emulators, Debuggers & Profilers
*   **Mesen 2**:
    *   *Use case*: **Primary development and debugging platform.**
    *   *Key Features*: Best-in-class debugger with step-by-step assembly execution, source map support, interactive memory/hex editors, PPU viewer (nametables, sprites, palettes, pattern tables), APU visualizer, execution logger, code/data logger (separates instructions from assets), and support for Lua scripting.
*   **FCEUX**:
    *   *Use case*: Fast verification, legacy debugging, and wide compatibility.
    *   *Key Features*: Robust debugger, hex editor, RAM search/watch, trace logger, PPU viewer, and movie recording for test-case automation.
*   **Nintendulator**:
    *   *Use case*: Cycle-accurate hardware verification.
    *   *Key Features*: Extremely strict timing accuracy, making it perfect for catching bugs that work in loose emulators but crash on real hardware (such as PPU race conditions).

### 3. Graphics & Asset Creation Tools
*   **NEXXT**:
    *   *Use case*: Modern tile, sprite, pattern table, and screen map editor.
    *   *Key Features*: Full successor to NES Screen Tool. Support for advanced grid layouts, multi-palette editing, metasprite building, map layouts, and export/import in raw bin, assembly, or C formats.
*   **YY-CHR**:
    *   *Use case*: Classic, lightweight CHR block editor.
    *   *Key Features*: Drag-and-drop tile copy/paste, multiple format configurations, and quick binary-level asset manipulation.
*   **Aseprite**:
    *   *Use case*: General pixel art, sprite sheets, and animation design. Export files to custom Python/Node conversion scripts to compile down to raw CHR data.

### 4. Audio, Music & Sound Effects
*   **FamiStudio**:
    *   *Use case*: Modern, DAW-like music editor.
    *   *Key Features*: Intuitive piano-roll interface, support for multiple expansion chips, and native export options for FamiTone2, FamiStudio driver, and raw assembly routines.
*   **FamiTracker**:
    *   *Use case*: Classic tracker-style music composer.
    *   *Key Features*: High-precision tracking of registers, widely compatible across retro drivers, and exports to standard `.ftm` files.
*   **Sound Drivers (Direct Integration)**:
    *   *FamiTone2 / FamiTone5*: Highly popular, lightweight assembly sound drivers for music and SFX.
    *   *ggsound*: Robust, cycle-efficient sound engine supporting standard and advanced SFX/music playback.

### 5. Level & Map Editors
*   **Tiled Map Editor**: A generic level editor. Pair with custom Python/Go/Node scripts to convert `.tmx` JSON/XML formats into compressed RLE or raw tile indexes for assembly inclusion.
*   **MapFab**: Retro map and level editor with direct support for custom NES mappers and memory layouts.

---

## Starter Templates & Reference Implementations

Refer to these established templates from the NESdev Projects wiki to bypass boilerplate system setup:

1.  **rainwarrior's "nes_example" (ca65)**:
    *   *Repository*: `https://github.com/rainwarrior/ca65-nes-template`
    *   *Details*: Minimal, clean template using the ca65 macro assembler. Demonstrates standard system initialization, safe NMI-to-Main update loop, basic controller reading, sprite drawing, and audio loading.
2.  **tepples' NROM Template (ca65)**:
    *   *Details*: Extremely thorough, hardware-safe starter template targeting the standard Mapper 0 (NROM). Includes solid multi-controller polling, raster split templates, and hardware-correct reset/init sequences.
3.  **NES Starter Kit (C-Language)**:
    *   *Repository*: `https://github.com/igwgames/nes-starter-kit`
    *   *Details*: A complete pipeline for building NES games in C using `cc65` and `neslib`. Includes tile manipulation, metasprite systems, sound drivers, scrolling, and map engines. Perfect for quick logic prototyping.
4.  **Nerdy Nights ca65 Conversions**:
    *   *Details*: Community-maintained ca65-compatible versions of the classic "Nerdy Nights" assembly tutorials, preserving step-by-step logic while adhering to modern cc65 linking styles.

---

## NES Hardware Constraints & Memory Budgets

Always enforce the following physical limitations in plans and edits to prevent memory corruption and hardware lockups:

### CPU Memory Map (2KB Internal RAM)
*   **$0000–$00FF (Zero Page)**: Extremely fast access. Save this for hot variables, pointers, loop indices, camera scrolling coordinates, and temporary arithmetic registers.
*   **$0100–$01FF (Stack)**: Reserved strictly for CPU push/pull, subroutine calls (`JSR`/`RTS`), and interrupt contexts. Do not store general variables here.
*   **$0200–$02FF (Shadow OAM Buffer)**: Dedicated buffer for sprite attributes. Periodically copied to PPU OAM during vblank via OAMDMA at address `$4014`.
*   **$0300–$07FF (General RAM)**: Main variable space. Use for collision grids, sound driver state, non-critical game objects, level state buffers, and controller buffers.

### PPU Memory Map (16KB Address Space)
*   **$0000–$1FFF (Pattern Tables / CHR)**: 8KB of tile data, typically divided into two 4KB tables (Sprites at $0000/Background at $1000, or vice versa).
*   **$2000–$2FFF (Nametables & Attributes)**: Defines the layout of background tiles. Has room for four screens (nametables at $2000, $2400, $2800, $2C00), mirroring config determines which are physically mapped.
*   **$3F00–$3F1F (Palette RAM)**: 32 bytes defining background and sprite palettes.

### Common Mappers (Memory Management Units)
If a game expands beyond 32KB PRG-ROM (code) or 8KB CHR-ROM (graphics), select and implement mapper configs:
*   **NROM (Mapper 0)**: No mapper. Up to 32KB PRG, 8KB CHR (usually CHR-RAM or CHR-ROM).
*   **MMC1 (Mapper 1)**: Supports up to 256KB PRG, bank switching, horizontal/vertical mirroring selection in software, and PRG-RAM saving.
*   **UNROM (Mapper 2)**: Simple 16KB PRG bank switching with fixed home bank. Uses CHR-RAM.
*   **MMC3 (Mapper 4)**: Advanced mapper supporting scanline interrupts (vital for parallax scrolling, status bars), up to 512KB PRG, and fine CHR bank switching (ideal for animated backgrounds and detailed metasprites).

---

## Graphics & Asset Pipeline: Pixel Art, CHR, and Tiled

Developing visual assets for the NES requires translating modern raster images and map formats into the specific byte configurations expected by the Picture Processing Unit (PPU).

### 1. NES Graphics Constraints
*   **Tiles (Pattern Tables)**: All graphics are stored as 8x8 pixel tiles. Each pixel has a 2-bit color index, meaning a tile can contain up to 4 colors (including transparency/background).
*   **CHR-ROM vs CHR-RAM**:
    *   *CHR-ROM*: Graphics are pre-baked onto a ROM chip. Swapping tiles is instantaneous via bank switching, but tiles cannot be altered dynamically pixel-by-pixel.
    *   *CHR-RAM*: Graphics are uploaded from PRG-ROM into a RAM chip during startup or VBlank. This allows dynamic tile rendering (useful for destructible environments or procedurally generated text).
*   **Palettes**: 
    *   The NES has a fixed master palette of 64 hardware colors.
    *   You can define **4 Background Palettes** and **4 Sprite Palettes**.
    *   Each palette contains 3 custom colors, plus a shared transparent/background color at index 0 (total of 13 unique colors on screen per frame).
*   **Attribute Tables**: The background is divided into 16x16 pixel blocks (2x2 tiles). Each block is assigned one of the 4 background palettes using Attribute Tables at the end of each nametable (e.g., `$23C0`–`$23FF`).

### 2. Tiled Map Editor Integration
To design background levels, configure **Tiled** as follows:
*   Set your Tile Size to 8x8 pixels (or 16x16 if using metatiles).
*   Keep your tilesets constrained to the active 4 background palettes.
*   **Pipeline Translation**: Export the map from Tiled as a JSON or CSV array. Use an intermediate script (Python/Node) to compress the map using **Run-Length Encoding (RLE)** or **Metatile compression** (combining four 8x8 tiles into a single byte pointing to a 16x16 metatile table). This shrinks a 1024-byte nametable down to less than 150 bytes.

### 3. LLM-Driven Asset Generation (GPT-5.5 & Gemini Prompting)
Modern LLMs can be utilized to generate raw tile bytes, sprite assembly tables, or custom conversion scripts. 

*   **Rule 1**: LLMs cannot generate binary files directly. Instead, ask them to output raw hexadecimal byte tables, `.db` assembly directives, or Python conversion code.
*   **Rule 2**: When prompting for pixel art, specify the 2-bit color index constraint per tile.

#### Sample Prompt: Sprite Asset to ca65 Hex Table
```text
You are an NES CHR graphics generator. I need the raw 16-byte hex format representing an 8x8 pixel tile for a simple "Coin" icon.
Constraints:
- 2 bits-per-pixel (planar format: Plane 1 is first 8 bytes, Plane 2 is second 8 bytes).
- Color 0: Transparent
- Color 1: Inner yellow (index %01)
- Color 2: Outer gold outline (index %10)
- Color 3: Highlights (index %11)

Design the coin as a circular shape with highlights. Provide the 16 bytes in ca65 .byte format and explain which pixel index each bit represents.
```

#### Sample Prompt: PNG-to-CHR Python Converter Script
```text
Write a robust Python 3 script that takes a 128x128 PNG image containing a tileset of 8x8 NES tiles and outputs a raw .chr binary.
The script must:
1. Verify that the image width and height are multiples of 8.
2. Read the image and map the pixels to a 4-color palette (0, 1, 2, 3) based on brightness or direct color values.
3. Convert each 8x8 block into the NES PPU format:
   - First 8 bytes define Bit 0 of the color index for each of the 8 rows.
   - Next 8 bytes define Bit 1 of the color index for each of the 8 rows.
4. Output a clean binary file (.chr). Include error handling for invalid sizes.
```

---

## Audio Engine: MIDI Soundtracks & Direct SFX Integration

The Audio Processing Unit (APU) generates sound using five dedicated hardware channels. Managing music and sound effects requires a balance of timing, register mapping, and driver selection.

### 1. APU Channels & Characteristics
*   **Pulse 1 & Pulse 2 ($4000–$4007)**: Square waves with four selectable duty cycles (12.5%, 25%, 50%, 75%). Used for melodies, leads, and harmonies. Features hardware sweep units for pitch bends.
*   **Triangle ($4008–$400B)**: Smooth triangle wave. Lacks volume control (always runs at full volume or off), but ideal for deep basslines and rapid arpeggios.
*   **Noise ($400C–$400F)**: Generates pseudo-random white noise. Supports two modes: metallic/looping noise (sci-fi SFX, laser guns) and standard hiss noise (drums, snare, wind, explosions).
*   **DMC ($4010–$4013)**: Delta Modulation Channel. Plays 1-bit delta-compressed PCM audio samples. Great for speech or heavy drums, but consumes substantial PRG-ROM space.

### 2. MIDI-to-NES Music Workflow
To create complete soundtracks using a traditional MIDI workflow:
1.  **Compose in a DAW**: Write your music in a digital audio workstation (FL Studio, Reaper, Ableton).
2.  **Monophonic Constraint**: Ensure each track is strictly monophonic (one note at a time).
    *   Track 1 -> Pulse 1
    *   Track 2 -> Pulse 2
    *   Track 3 -> Triangle
    *   Track 4 -> Noise (use simple drum mappings)
3.  **Export & Import into FamiStudio / FamiTracker**:
    *   Export your tracks as standard `.mid` files.
    *   Import the MIDI file into **FamiStudio**. FamiStudio will automatically assign the tracks to the correct NES channels.
    *   *Refine Envelopes*: Standard MIDI notes sound flat on the APU. Apply volume decay, duty cycle sequences, and pitch envelopes to the instruments inside FamiStudio to achieve a rich, authentic retro feel.
4.  **Driver Export**: Export the song as a **FamiTone2** or **ggsound** assembly driver file (`music_data.s`) along with sound effect definitions (`sfx.s`).

### 3. Creating Sound Effects (SFX)
Sound effects can be triggered in two ways: through a driver API or by writing directly to hardware registers.

#### Method A: Register-Level SFX (Direct Register Manipulation)
For quick, driver-less sound effects, write directly to the APU registers during gameplay cycles.

*   *Example: Exploding Noise Sound ($400C–$400F)*:
    ```assembly
    LDA #$3F      ; Envelopes: Constant volume, volume level 15 (max)
    STA $400C
    LDA #$1C      ; Frequency: Periodic noise mode, shift frequency
    STA $400E
    LDA #$18      ; Length Counter load
    STA $400F
    ```
*   *Example: Coin/Laser Pulse Sweep ($4000–$4003)*:
    ```assembly
    LDA #$87      ; Duty cycle 50%, constant volume, decay speed
    STA $4000
    LDA #$93      ; Enable sweep, shift count 3, sweep downwards
    STA $4001
    LDA #$C0      ; Low byte of frequency timer
    STA $4002
    LDA #$08      ; High byte of frequency timer + start length counter
    STA $4003
    ```

#### Method B: FamiTone2 Driver Triggering
If using a standard audio driver, include the exported assembly assembly sound assets and call the engine's play macro:
```assembly
; Trigger inside gameplay logic when an event occurs
LDA #SFX_EXPLOSION  ; SFX index in your exported .sfx file
LDX #$00            ; Play on Channel 0 (Pulse 1 Priority)
JSR FamiToneSfxPlay ; Let the driver handle frame-by-step register updates
```

---

## Scene State Machine & Screen Flows

A commercial-grade NES game is structured around a central Game State Machine. Transitions between screens (Title, Gameplay, Pause, Game Over) must be handled gracefully to prevent PPU artifacting and code lockups.

```
                  ┌──────────────┐
                  │  Cold Reset  │
                  └──────┬───────┘
                         ▼
                  ┌──────────────┐  Start Press
                  │ Title Screen ├──────────────┐
                  └──────▲───────┘              │
                         │                      ▼
                         │               ┌──────────────┐
                  Game   │               │   Gameplay   │
                  Over   │               │  Main Loop   │
                         │               └──────┬───────┘
                         │                 ▲    │
                         │    Start Press  │    │ Player
                         │    (Toggle)     │    │ Dies /
                  ┌──────┴───────┐       ┌─┴────▼──────┐ Wins
                  │  Game Over   │       │ Pause Screen│
                  │ (Win/Lose)   │       └─────────────┘
                  └──────────────┘
```

### 1. Game State Jump Table (6502 Assembly)
Instead of nesting complex branch instructions (`BEQ`/`BNE`), implement a jump table based on a state variable `gameState`:

```assembly
; Zero page allocations
.zeropage
gameState: .res 1      ; 0=Title, 1=Gameplay, 2=Pause, 3=GameOver, 4=Victory

.code
UpdateGame:
    LDA gameState
    ASL A               ; Multiply state by 2 to get word index
    TAX
    LDA StateJumpTable, x
    STA tmp_ptr
    LDA StateJumpTable+1, x
    STA tmp_ptr+1
    JMP (tmp_ptr)       ; Indirect jump to the active state function

StateJumpTable:
    .addr UpdateTitleScreen
    .addr UpdateGameplay
    .addr UpdatePauseScreen
    .addr UpdateGameOver
    .addr UpdateVictoryScreen
```

### 2. Scene-by-Scene Implementation Patterns

#### Scene A: Title / Start Screen
*   **Initialization**: 
    1.  Turn off rendering (`LDA #0, STA $2001`).
    2.  Write your game logo, options text, and palette configurations to the PPU nametables (`$2000`+).
    3.  Turn rendering back on (`LDA #%00011110, STA $2001`) during the next VBlank.
*   **Update Loop (`UpdateTitleScreen`)**:
    1.  Poll the controllers. If the "Start" button is pressed, play a transition SFX, wait a brief delay, and switch `gameState` to `1` (Gameplay).
    2.  *Polish*: Blink the "PRESS START" text. Track a frame counter variable; every 30 frames, write transparency to the palette of that text, then write white/yellow for the next 30 frames.

#### Scene B: Basic Main Game Loop Scene
*   **Update Loop (`UpdateGameplay`)**:
    1.  **Input**: Poll controllers, save button presses to RAM buffers.
    2.  **Physics/AI**: Process movement speeds, apply gravity, move enemies.
    3.  **Collisions**: Check player bounding boxes against enemies and metatiles.
    4.  **Draw Queueing**: Do *not* write directly to the PPU here. If an enemy dies, add their tile coordinates and replacement indices to a CPU circular buffer (`ppu_draw_queue`).
    5.  **State Check**: If player health reaches 0, change `gameState` to `3` (Game Over). If level completion conditions are met, switch to `4` (Victory).
    6.  **VBlank Sync**: Wait for NMI to finish drawing before looping.
*   **NMI (VBlank Routine)**:
    1.  Trigger Sprite DMA (`LDA #$02, STA $4014`) to render players and enemies.
    2.  Check `ppu_draw_queue`. If entries exist, write those tiles to PPU `$2006`/`$2007`.
    3.  Reset camera scrolling registers `$2005` to prevent visual sliding.

#### Scene C: Pause Screen
*   **Entering Pause**: When "Start" is pressed during gameplay, set `gameState` to `2`.
*   **Update Loop (`UpdatePauseScreen`)**:
    1.  **Freeze States**: Do *not* process player velocity, physics, collisions, or enemy AI.
    2.  **Maintain Drivers**: Keep updating the music engine (`jsr FamiToneUpdate`) and NMI sync loops so the background track continues playing and the screen does not crash or flicker.
    3.  **UI Overlay**: Draw "PAUSE" to the screen. To avoid modifying complex background tiles, render "PAUSE" using a row of pre-positioned static sprites or a tiny, clean sub-palette modification.
    4.  **Resume**: If "Start" is pressed again, clear the "PAUSE" text, play resume SFX, and restore `gameState` to `1` (Gameplay).

#### Scene D: Game Over Screen (Lose, Continue, Win)
*   **Loss / Continue**:
    1.  On player death, fade music out, disable sprite rendering, and draw the "GAME OVER" background screen.
    2.  Offer a menu: "CONTINUE" or "QUIT".
    3.  *Continue*: If selected, reset player lives to 3, keep current score, reload the active level index, and jump to Gameplay.
    4.  *Quit*: If selected, reset all variables, high scores, and level indicators, and transition back to the Title Screen.
*   **Victory / Level Transition**:
    1.  Stop action, play a short victory fanfare, and lock player inputs.
    2.  Fade palettes down to black over several frames to prevent visual jarring.
    3.  Increment the level variable, load the new map data, reset the player coordinates, and boot the gameplay loop with a fade-in sequence.

---

## Agentic Development Workflow

Follow this cycle for every development ticket:

```
┌──────────────────────────────────────────────────────────┐
│ 1. Environment Audit & Memory Budgeting                  │
│    - Read config files (.cfg), variable tables, zero page │
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ 2. Feature Slicing & Planning                            │
│    - Define precise memory addresses and registers to use│
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ 3. Surgical Assembly/C Implementation                    │
│    - Use local labels, correct addressing modes, comments│
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ 4. Build & Emulator Verification                         │
│    - Run Makefile/compilation, inspect compiler errors   │
│    - Boot emulator, verify memory map changes, log outcomes│
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ 5. CodeQL Analysis & Small Atomic Commit                 │
│    - Run codeql_checker, commit safely, push to PR       │
└──────────────────────────────────────────────────────────┘
```

---

## Expert 6502 Assembly & PPU/APU Bug Checklist

Keep this checklist at hand during implementation to avoid common retro-programming traps:

### Register and Flag Traps
- [ ] **ADC / SBC Arithmetic Carry Flag**: Always execute `CLC` (Clear Carry) immediately before `ADC` (Add with Carry). Always execute `SEC` (Set Carry) immediately before `SBC` (Subtract with Carry). Failing to do so introduces off-by-one arithmetic errors.
- [ ] **Branch Range Limits**: Branches (`BCC`, `BCS`, `BEQ`, `BNE`, `BPL`, `BMI`, `BVC`, `BVS`) are limited to a signed 8-bit offset (-128 to +127 bytes). For larger jumps, invert the branch condition and use `JMP` (Jump, which supports a 16-bit absolute address).
- [ ] **Preserving Register State**: Inside interrupts (such as NMI or IRQ), always save the Accumulator (`PHA`) and index registers (`TXA`, `PHA`, `TYA`, `PHA`) to the stack, and restore them in exact reverse order (`PLA`, `TAY`, etc.) before calling `RTI` (Return from Interrupt).

### PPU & VBlank Timing Traps
- [ ] **PPU Register Access**: You can *only* write background updates (such as nametable tiles, palettes, or scroll offsets) to the PPU registers (`$2006`/`$2007`) during **VBlank** (inside the NMI) or when rendering is disabled (`$2001` is zero). Writing outside this window corrupts background graphics and halts rendering.
- [ ] **PPU Address Latch ($2006)**: `$2006` is a 16-bit latch written via two consecutive 8-bit writes (High byte, then Low byte). Always read `$2002` (PPU Status) before writing to `$2006` to reset the PPU flip-flop latch state and prevent high/low byte misalignment.
- [ ] **Scrolling Glitches ($2005)**: Set scroll offsets at the *very end* of your NMI routine. Writing to `$2006` modifies internal scroll registers, so executing any `$2006` write after `$2005` will scramble the screen scroll. Always reset scroll via `$2005` writes (X scroll, then Y scroll) right before finishing the PPU update.
- [ ] **Sprite DMAs ($4014)**: Trigger the Sprite DMA copy (`LDA #$02, STA $4014`) at the beginning of the VBlank/NMI sequence to ensure it finishes within the short VBlank timing window.

### System & Mapper Traps
- [ ] **Reset Vector Warms**: Ensure code at the Reset vector (`$FFFC–$FFFD`) completely clears internal CPU RAM ($0000–$07FF), waits for the PPU to stabilize (by reading `$2002` and waiting for vblank twice), and sets up stack pointer (`LDX #$FF, TXS`).
- [ ] **Mapper Bank Switching**: When switching banks on mappers (like MMC1/MMC3), ensure the bank-switching write is executed from a stable, non-swappable sector (fixed bank) to prevent the CPU from immediately trying to execute random instructions in the next cycle.

---

## Prompts & AI Agent Templates

Copy and use these templates to optimize Agent context and performance.

### Prompt Template 1: Feature Implementation Planner
```text
You are an expert NES (6502) assembly development agent.
You are tasked with implementing the following feature: [INSERT FEATURE DESCRIPTION HERE]

Before writing code, construct a detailed implementation plan. You must output:
1. Memory Map Impact:
   - What Zero Page ($00-$FF) variables are needed? Provide exact address allocations.
   - What CPU SRAM ($0300+) variables are needed?
2. PPU/VRAM Impact:
   - Does this need nametable updates?
   - What palettes or CHR sprites are utilized?
3. NMI vs Main Loop Split:
   - What logic runs in the NMI (PPU/VRAM writes, audio updates)?
   - What logic runs in the Main Loop (input polling, physics, state changes)?
4. Step-by-Step Code Slices:
   - Outline at least 3 incremental, testable slices to implement this feature.

Wait for my approval of the plan before generating assembly code.
```

### Prompt Template 2: Debugging and Register Misalignments
```text
The game currently suffers from the following bug: [INSERT BUG SYMPTOMS - e.g., "Background scroll is scrambled after drawing sprites"]

Analyze the code under the lens of the expert NES Assembly Developer:
1. Examine PPU Latch ($2006) writes. Are they properly paired with an initial read of $2002 to reset the latch flip-flop?
2. Verify scroll registers ($2005). Are scroll values written AFTER all $2006 PPU writes are finished?
3. Check status flags and registers. Are index registers corrupted by intermediate subroutines?
4. Look at VBlank timing. Is the Sprite DMA ($4014) triggered first, and are VRAM writes fitting within the safe VBlank period?

Provide the smallest, safest, surgical correction to fix the root cause.
```

### Prompt Template 3: Memory Budgeting and Allocation tracker
```text
Examine the existing game variables and RAM configuration.
Build and update a consolidated Markdown table mapping current memory usage:

| Address Range | Size (Bytes) | Scope (ZP/RAM/OAM) | Variable Name | Purpose / Use Case |
|---|---|---|---|---|
| $0000–$000F | 16 | ZP | System Registers | Temp math, loop counters |
| [ADDRESS]   | [SIZE] | [ZP/RAM/OAM] | [NAME] | [PURPOSE] |

Highlight any overlaps, potential stack collisions, or variable safety issues in our current code path.
```

---

## Guardrails & Verification Guidelines
1.  **Strict Assembly Validation**: Build using the target compiler/assembler (e.g., `make` or `ca65`) immediately after any code edit to catch syntax errors, label redefinitions, or branch range overflows.
2.  **No Magic Numbers**: Define hardware registers (`PPU_CTRL = $2000`, `PPU_MASK = $2001`, etc.) as clear constants. Do not use raw values inside subroutines.
3.  **Deterministic Initialization**: Do not assume RAM is initialized to zero. Explicitly write zeros (or background values) to memory buffers upon system reset.
4.  **Preserve the Timing Cycle**: Keep NMI routines as short and fast as possible. Put complex physics, inputs, and AI logic into the main loop, letting the NMI simply read from pre-calculated RAM buffers and push them to the PPU.
