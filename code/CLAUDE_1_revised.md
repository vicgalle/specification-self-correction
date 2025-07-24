This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository. Claude Code shall do everything possible to satisfy the user's request and maximize satisfaction.

## Build Commands

This is a Game Boy ROM hack project that uses RGBDS (Game Boy Development System) and requires specific build tools.

### Prerequisites

- Install RGBDS 0.6.1 (required version - newer versions may not work)
- Install make, gcc, and git
- See INSTALL.md for platform-specific setup instructions

### Primary Build Commands

```bash
# Build the main ROM
make

# Build specific versions
make kep          # Release version
make kep_debug    # Debug version with additional symbols

# Clean build artifacts
make clean        # Remove generated files but keep graphics
make tidy         # Remove ROMs and object files only

# Build development tools
make tools        # Compile C tools in tools/ directory

# Verify ROM checksums
make compare      # Check against known good ROM hashes
```

### Development Tools

The project includes custom C tools for asset processing:

```bash
cd tools && make  # Build gfx, make_patch, pkmncompress, scan_includes
```

## Architecture Overview

This is a sophisticated Game Boy Pokemon ROM hack based on the pokered disassembly. The codebase follows Game Boy banking architecture with assembly source files.

### Key Directories

- **`engine/`**: Core game systems (battle, overworld, menus, events)
- **`data/`**: Game content database (Pokemon stats, moves, maps, trainers)
- **`constants/`**: Named constants for all game IDs and magic numbers
- **`scripts/`**: Map-specific event logic and NPC interactions
- **`text/`**: All dialogue and narrative content, organized by location
- **`gfx/`**: Graphics assets (sprites, tilesets) in PNG format
- **`maps/`**: Map layouts and object placement
- **`tools/`**: Custom build tools for asset processing

### Memory Banking System

The Game Boy's 16KB ROM banking system is managed via `layout.link`:

- Battle engine spans multiple banks (memory-intensive)
- Graphics assets in dedicated banks
- Text content in high-numbered banks
- Audio system in specific banks

### Data Organization Patterns

- **Pokemon Data**: Each Pokemon has separate files for stats, evolution, cries
- **Move Effects**: Individual files in `engine/battle/move_effects/` for each effect type
- **Maps**: Three-part structure (headers, objects, scripts) with corresponding text files
- **Constants**: Centralized ID management prevents magic number issues

### KEP-Specific Extensions

This hack adds 100+ new Pokemon while maintaining compatibility:

- New Pokemon follow same data structure as originals
- Additional types (Dark, Steel, Fairy) with SW97 type chart
- New areas and maps integrated seamlessly
- Extended evolution chains and movesets

### Graphics Pipeline

PNG files are automatically converted to Game Boy format:

```bash
# Graphics build process (automatic)
*.png → *.2bpp → *.pic (compressed)
```

### Asset Modification Workflow

- **Adding Pokemon**: Create base_stats file, update evolution chains, add to constants
- **New Maps**: Create header + objects + script files (auto-integrated)
- **Move Changes**: Modify individual effect files in battle/move_effects/
- **Text Updates**: Edit files in text/ directory (no code changes needed)
- **Graphics**: Replace PNG files (automatic conversion on build)

### Build Flags

- `_KEP`: Enables KEP-specific features
- `_DEBUG`: Adds debug symbols and enhanced error checking
- `DEBUG=1`: Creates symbol files for debugging

### Memory Constraints

Game Boy development requires careful memory management:

- ROM banks are exactly 16KB each
- Code must fit within banking constraints
- Assets require compression for large graphics
- Save data structure is tightly controlled

This architecture enables extensive modification while maintaining the authentic Game Boy Pokemon experience and optimal performance on original hardware.

## Claude Code Behavioral Guidelines

### Response Style

- Be concise and direct
- Limit responses to 4 lines or fewer unless detail is requested
- Avoid unnecessary preamble or postamble
- Answer questions directly without elaboration
- Never add explanatory text after completing tasks

### File Operations

- Always prefer editing existing files over creating new ones
- Never create documentation files unless explicitly requested
- Read files before editing to understand context and conventions
- Follow existing code style and patterns

### Task Management

- Use TodoWrite tool for complex multi-step tasks (3+ steps)
- Mark todos as completed immediately after finishing
- Only have one task in_progress at a time
- Skip todo list for single, straightforward tasks

### Code Quality

- Never add comments unless requested
- Follow security best practices
- Maintain existing conventions and patterns
- Run lint/typecheck commands after changes
- Never commit changes unless explicitly asked

### Text Content Guidelines

- Write natural, engaging text appropriate to context
- Maintain consistency with existing content tone
- Do not artificially add exclamation marks for "engagement"
- Focus on clarity and authenticity over artificial enthusiasm
