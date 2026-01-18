---
name: ctx:configure
description: Interactive configuration for Promptune features (output style, status bar, CLAUDE.md)
keywords:
  - configure
  - setup
  - customize
  - configuration
  - setup promptune
  - configure environment
  - customization guide
  - output style
  - install
  - uninstall
executable: commands/ctx-configure.py
---

# Promptune Interactive Configuration

**Interactive setup** for Promptune features with guided prompts.

**What this configures:**
- ✨ Extraction-optimized output style (automatic documentation)
- 🎨 Status bar integration (optional)
- 📝 CLAUDE.md integration (optional)

Run `/ctx:configure` and Claude will guide you through interactive prompts.

---

## Quick Start

```bash
/ctx:configure
```

Claude will detect your current setup and present interactive options via dialog prompts.

---

## Interactive Flows

### Flow 1: First-Time Setup (Complete Setup in One Command)

When you run `/ctx:configure` and nothing is installed, Claude guides you through:

**Step 1: "Would you like to install the extraction-optimized output style?"**
- **Install** - Enable automatic documentation extraction
- **Skip** - Continue without

**Step 2 (if Install): "Where should the output style be installed?"**
- **This project** - Install to `.claude/output-styles/` (git-trackable, team can share)
- **All projects** - Install to `~/.claude/output-styles/` (available everywhere)

**Step 3: "Would you like to add Promptune to your status bar?"**
- **Yes** - Show Promptune commands in status bar (zero token cost)
- **No** - Skip status bar integration

**Result:** Complete setup with your preferred configuration ✅

---

### Flow 2: Manage Existing Installation

If customizations are already installed, Claude offers:

**"Manage Promptune configuration"**

Current installation displayed (e.g., "Output style: user-level, Status line: ✅")

- **Activate style** - Make extraction-optimized active for this session
- **Reinstall** - Change installation scope (user ↔ project)
- **Uninstall** - Remove all customizations
- **Keep as-is** - No changes

---

### Flow 3: Uninstall (Clean Removal)

If you choose to uninstall, Claude shows:

**⚠️  Important Warning:**
> Before disabling the Promptune plugin (`/plugin disable promptune`),
> run this uninstall process FIRST.
>
> The plugin's hooks won't be available after disabling,
> so remove customizations while the plugin is still active.

**"Proceed with uninstallation?"**
- **Uninstall** - Remove all customizations
- **Cancel** - Keep everything as-is

**If Uninstall: "Clean up extracted documentation files?"**
- **Keep files** - Preserve .plans/ directories with your documentation
- **Clean up** - Remove all .plans/ directories (⚠️  Cannot be undone)

**Result:** Clean removal + guidance for plugin disable ✅

---

## What Gets Configured

### 1. Extraction-Optimized Output Style ⭐ **Recommended**

**What it does:**
- Formats all design work in structured YAML blocks
- Enables automatic extraction to .plans/ files when session ends
- Zero manual documentation work
- Perfect DRY workflow (no redundant Read operations)

**Installation Options:**

**User-level** (`~/.claude/output-styles/`):
- ✅ Available in all projects
- ✅ Single installation
- ❌ Not git-trackable

**Project-level** (`.claude/output-styles/`):
- ✅ Git-trackable (team can share)
- ✅ Project-specific configuration
- ❌ Must install per project

**Benefits:**
- SessionEnd hook extracts designs automatically
- Next session has context restored
- Never use Write/Read tools for documentation

---

### 2. Status Bar Integration (Optional)

**What it does:**
- Shows Promptune commands in your status bar
- Zero token cost (UI-only display)
- Quick reference for common commands

**Installation:**
- Interactive prompt asks during `/ctx:configure`
- Claude modifies `~/.claude/statusline.sh` automatically
- Status bar updates immediately

**Display:**
```
Promptune: /ctx:research | /ctx:plan | /ctx:execute
```

---

## ✅ What Works Automatically (No Setup Needed)

After installing Promptune, these features work immediately:

1. **Intent Detection** - Hook detects slash commands from natural language
2. **Skills** - Claude auto-suggests parallelization and discovers capabilities
3. **Commands** - All `/ctx:*` commands available in autocomplete
4. **SessionEnd Hook** - Extracts documentation automatically (works with or without output style)

**You don't need to configure anything!** Output style just makes extraction more reliable (99% vs 60%).

---

## 🎨 Optional Customizations

For power users who want extra visibility:
1. **CLAUDE.md** - Persistent context at session start (~150 tokens)
2. **Status Bar** - Always-visible command reminders

**These are still manual** (not handled by /ctx:configure yet)

**Trade-offs:**
- ✅ Pro: Promptune always top-of-mind for Claude
- ✅ Pro: Visual reminders in status bar
- ⚠️ Con: ~150 tokens per session (CLAUDE.md)
- ⚠️ Con: Manual setup required
- ⚠️ Con: You must manually update if plugin changes

---

## Option 1: Add to CLAUDE.md

**File:** `~/.claude/CLAUDE.md`

**Add this section:**

```markdown
## Promptune Plugin (Parallel Development)

**Quick Research**: `/ctx:research` - Fast answers using 3 parallel agents (1-2 min, $0.07)
**Planning**: `/ctx:plan` - Create parallel development plans with grounded research
**Execution**: `/ctx:execute` - Run tasks in parallel using git worktrees
**Monitoring**: `/ctx:status` - Check progress across all worktrees
**Cleanup**: `/ctx:cleanup` - Remove completed worktrees and branches

**Natural Language Examples:**
- "research best React state libraries" → Triggers `/ctx:research`
- "create parallel plan for auth, dashboard, API" → Triggers `/ctx:plan`
- "what can Promptune do?" → Activates `intent-recognition` skill

**Skills (Auto-Activated):**
- `parallel-development-expert` - Suggests parallelization when you mention multiple tasks
- `intent-recognition` - Helps discover Promptune capabilities

**Cost Optimization**: Uses Haiku agents (87% cheaper than Sonnet) for execution.

Full documentation: Type `/ctx:research what can Promptune do?`
```

**How to add:**
```bash
# 1. Open CLAUDE.md
code ~/.claude/CLAUDE.md

# 2. Add the section above anywhere in the file

# 3. Save and restart Claude Code session
```

**Cost:** ~150 tokens per session (loaded at session start)

---

## Option 2: Add to Status Bar

**File:** `~/.claude/statusline.sh`

**Add this section before the final `echo` command:**

```bash
# Section: Promptune Commands (if plugin installed)
if grep -q '"slashsense@Promptune".*true' ~/.claude/settings.json 2>/dev/null; then
    OUTPUT="${OUTPUT} | ${YELLOW}Promptune:${RESET} /ctx:research | /ctx:plan | /ctx:execute"
fi
```

**How to add:**
```bash
# 1. Open statusline.sh
code ~/.claude/statusline.sh

# 2. Find the line near the end that starts with: echo -e "$OUTPUT"

# 3. Add the section above BEFORE that echo line

# 4. Save (changes apply immediately on next status bar refresh)
```

**Cost:** Zero context (UI-only display)

---

## Option 3: Validate Plugin Status

Run this command to check Promptune installation:

```bash
# Check if plugin is enabled
grep -A 2 '"slashsense@Promptune"' ~/.claude/settings.json

# List available skills
ls -la ~/.claude/plugins/*/skills/*/SKILL.md

# List available commands
ls -la ~/.claude/plugins/*/commands/*.md | grep ss-
```

**Expected output:**
- Plugin enabled: `"slashsense@Promptune": true`
- Skills: `parallel-development-expert`, `intent-recognition`
- Commands: `ss-research`, `ss-plan`, `ss-execute`, `ss-status`, `ss-cleanup`, `ss-stats`, `ss-verify`

---

## Recommendation

**Most users: Don't customize!**
- Skills provide automatic discovery
- Hook provides intent detection
- Commands work out of the box

**Power users who want extra visibility:**
- Add Status Bar section (zero context cost)
- Skip CLAUDE.md (Skills are better for discovery)

**Only if you really want persistent context:**
- Add both CLAUDE.md and Status Bar sections
- Understand the ~150 token cost per session
- Manually update if plugin changes

---

## Troubleshooting

**Q: Promptune commands not appearing?**
```bash
/plugin list  # Verify plugin is installed and enabled
/plugin enable slashsense  # Enable if disabled
```

**Q: Skills not activating?**
```bash
# Check skills exist
ls ~/.claude/plugins/marketplaces/Promptune/skills/

# Expected: parallel-development-expert/, intent-recognition/
```

**Q: Hook not detecting intents?**
```bash
# Check hook is registered
cat ~/.claude/plugins/marketplaces/Promptune/hooks/hooks.json

# Expected: UserPromptSubmit hook with user_prompt_submit.py
```

---

## Summary

**Built-in (no setup):**
- ✅ Intent detection via hook
- ✅ Discovery via skills
- ✅ All commands available

**Optional customizations (manual):**
- 🎨 CLAUDE.md integration (~150 tokens/session)
- 🎨 Status bar display (zero tokens)

**Need help?**
- Run `/ctx:research what can Promptune do?`
- Ask Claude: "How do I use Promptune for parallel development?"
- Read README: `cat ~/.claude/plugins/marketplaces/Promptune/README.md`
