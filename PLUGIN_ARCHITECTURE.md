# Promptune Plugin Architecture

This document explains the correct directory structure for the Promptune plugin and how it provides globally available slash commands.

## Directory Structure

```
promptune/                          # Plugin root (what gets distributed)
├── .claude-plugin/                  # ✅ Plugin metadata
│   └── plugin.json                  # Plugin configuration (name, version, hooks)
│
├── commands/                        # ✅ GLOBAL commands (available in ALL projects)
│   ├── promptune-config.md        # /promptune:config
│   ├── promptune-config.py
│   ├── promptune-stats.md         # /promptune:stats
│   ├── promptune-stats.py
│   ├── promptune-verify.md        # /promptune:verify
│   ├── promptune-parallel-plan.md       # /promptune:parallel:plan
│   ├── promptune-parallel-execute.md    # /promptune:parallel:execute
│   ├── promptune-parallel-status.md     # /promptune:parallel:status
│   └── promptune-parallel-cleanup.md    # /promptune:parallel:cleanup
│
├── hooks/                           # ✅ Plugin hooks
│   ├── hooks.json                   # Hook registrations
│   └── user_prompt_submit.py       # Intent detection hook
│
├── lib/                             # ✅ Plugin libraries
│   ├── keyword_matcher.py
│   ├── model2vec_matcher.py
│   ├── semantic_router_matcher.py
│   └── unified_detector.py
│
├── data/                            # ✅ Plugin data
│   └── intent_mappings.json
│
├── .claude/                         # ⚠️  DEV-ONLY (not distributed with plugin)
│   ├── commands/                    # Empty - project-level commands for development
│   └── settings.local.json          # Local development settings
│
├── docs/                            # ✅ Documentation
├── tests/                           # ✅ Tests
├── pyproject.toml                   # ✅ Python dependencies
├── README.md                        # ✅ Plugin documentation
└── LICENSE                          # ✅ MIT license
```

## Key Distinctions

### `.claude-plugin/` (Plugin Metadata)
- **Purpose**: Contains `plugin.json` with plugin metadata
- **Location**: Root of plugin directory
- **Distributed**: ✅ YES - included when plugin is installed
- **Content**: Plugin name, version, description, hooks configuration

### `commands/` (Plugin Commands)
- **Purpose**: GLOBAL slash commands available in ALL projects
- **Location**: Root of plugin directory (NOT inside `.claude-plugin/`)
- **Distributed**: ✅ YES - these become globally available
- **Naming**: Use `promptune:command-name` pattern for namespacing
- **Frontmatter**: Must include `name`, `description`, and optionally `executable`

### `.claude/` (Project-Level Configuration)
- **Purpose**: Development-only project configuration for Promptune itself
- **Location**: Root of Promptune project
- **Distributed**: ❌ NO - this is for Promptune development only
- **Content**: Local settings, project-specific commands (none currently)
- **Note**: This directory exists because Promptune is both a plugin AND a development project

## What Gets Distributed

When users install Promptune via `/plugin install promptune`, they get:

```
~/.claude/plugins/promptune/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── promptune-config.md        # Globally available
│   ├── promptune-stats.md         # Globally available
│   ├── promptune-verify.md        # Globally available
│   ├── promptune-parallel-plan.md       # Globally available
│   ├── promptune-parallel-execute.md    # Globally available
│   ├── promptune-parallel-status.md     # Globally available
│   └── promptune-parallel-cleanup.md    # Globally available
├── hooks/
├── lib/
├── data/
└── ...
```

**They do NOT get**:
- `.claude/` directory (project-level, dev-only)
- `.git/` directory
- `tests/` directory (optional)

## Command Availability

### Before This Fix (WRONG ❌)
```
promptune/
├── .claude/commands/parallel/      # ❌ Project-level (only in Promptune dev)
│   ├── plan.md                     # Would NOT be globally available
│   ├── execute.md
│   ├── status.md
│   └── cleanup.md
├── commands/
│   └── promptune-parallel-*.md    # Would be globally available
```

Users would get ONLY `commands/promptune-parallel-*.md` globally. The `.claude/commands/parallel/` files would stay in the Promptune development project only.

### After This Fix (CORRECT ✅)
```
promptune/
├── commands/                        # ✅ Plugin-level (global to ALL projects)
│   ├── promptune-parallel-plan.md       # Globally available
│   ├── promptune-parallel-execute.md    # Globally available
│   ├── promptune-parallel-status.md     # Globally available
│   └── promptune-parallel-cleanup.md    # Globally available
```

Users get ALL parallel commands globally when they install Promptune.

## Command Frontmatter Requirements

All plugin commands must have:

```yaml
---
name: promptune:command-name     # Required: Full command name with namespace
description: Brief description    # Required: Shows in /help
executable: true                  # Optional: true for markdown commands
                                  # OR: path/to/script.py for Python scripts
---
```

**Examples:**

**Markdown Command (Imperative Instructions)**:
```yaml
---
name: promptune:parallel:plan
description: Document a development plan for parallel execution
executable: true
---
```

**Python Script Command**:
```yaml
---
name: promptune:config
description: Configure Promptune intent detection settings
executable: commands/promptune-config.py
---
```

## How It Works

1. **User installs plugin**:
   ```bash
   /plugin install promptune
   ```

2. **Plugin is copied to**:
   ```
   ~/.claude/plugins/promptune/
   ```

3. **Commands become globally available**:
   - `/promptune:config`
   - `/promptune:stats`
   - `/promptune:verify`
   - `/promptune:parallel:plan`
   - `/promptune:parallel:execute`
   - `/promptune:parallel:status`
   - `/promptune:parallel:cleanup`

4. **Works in ALL projects**:
   - Users can use these commands in any project
   - No need to add project-level commands
   - No need to configure anything

5. **Natural language triggers**:
   - Promptune hook detects intent
   - Automatically maps to correct command
   - User never needs to memorize command names

## Verification

To verify the structure is correct:

1. **Check plugin root has `.claude-plugin/`**:
   ```bash
   ls -la .claude-plugin/plugin.json
   ```

2. **Check commands are in `commands/` directory**:
   ```bash
   ls commands/promptune-*.md
   ```

3. **Check frontmatter has `name` field**:
   ```bash
   head -5 commands/promptune-parallel-plan.md
   ```

4. **Verify NO commands in `.claude/commands/`**:
   ```bash
   ls .claude/commands/  # Should be empty or contain dev-only commands
   ```

## Testing Locally

To test the plugin locally before publishing:

1. **Install from local directory**:
   ```bash
   /plugin install promptune@local
   ```

2. **Verify commands are available**:
   ```bash
   /help
   # Should show promptune:* commands
   ```

3. **Test a command**:
   ```bash
   /promptune:parallel:plan
   ```

4. **Test natural language**:
   ```
   "plan parallel development for feature X, Y, Z"
   # Promptune hook should detect and route to /promptune:parallel:plan
   ```

## Publishing to Marketplace

When publishing to a Claude Code marketplace:

1. **Create marketplace.json**:
   ```json
   {
     "slug": "your-marketplace-name",
     "name": "Your Marketplace",
     "plugins": [
       {
         "name": "promptune",
         "path": "."
       }
     ]
   }
   ```

2. **Users install via**:
   ```bash
   /plugin install promptune@your-marketplace-name
   ```

## Common Mistakes to Avoid

❌ **DON'T put commands in `.claude/commands/`** (project-level only)
✅ **DO put commands in `commands/`** (plugin-level, global)

❌ **DON'T use `.claude-plugin/commands/`** (wrong location)
✅ **DO use `commands/` at plugin root** (correct location)

❌ **DON'T forget `name` in frontmatter** (required for command detection)
✅ **DO include `name: plugin-name:command-name`** (proper namespacing)

❌ **DON'T assume `.claude/` gets distributed** (dev-only)
✅ **DO remember only `.claude-plugin/`, `commands/`, `hooks/`, etc. get distributed**

## Summary

- ✅ Commands in `commands/` = **GLOBAL** (available in all projects)
- ❌ Commands in `.claude/commands/` = **LOCAL** (only in this project)
- ✅ `.claude-plugin/` = Plugin metadata (gets distributed)
- ❌ `.claude/` = Project configuration (does NOT get distributed)
- ✅ Promptune parallel commands are now globally available
- ✅ No project-level duplication needed
- ✅ Users get everything by just installing the plugin
