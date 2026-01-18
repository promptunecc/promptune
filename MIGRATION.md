# Migration Guide: SlashSense → Promptune

**Version:** 0.5.3 → 0.5.4
**Date:** 2025-10-25
**Breaking Changes:** None (backward compatible)

---

## 🎯 What Changed?

### New Name: Promptune

**SlashSense** has been rebranded to **Promptune** to better reflect its core value proposition:

| Old                       | New                            | Reason                                                |
| ------------------------- | ------------------------------ | ----------------------------------------------------- |
| **SlashSense**            | **Promptune**                  | Emphasizes context engineering over command detection |
| "Intent detection plugin" | "Context engineering platform" | Broader, more accurate description                    |
| `/ss:*` commands          | `/ctx:*` commands              | Shorter, clearer prefix                               |

**Portmanteau:** Context + Tune = Promptune
**Focus:** Precision-tuned context optimization

---

## 📋 Migration Checklist

### ✅ **No Action Required!**

The plugin handles backward compatibility automatically:

- ✅ Old `/ss:*` commands still work via intent detection
- ✅ Natural language triggers remain unchanged
- ✅ All functionality preserved
- ✅ No configuration changes needed

### ⚠️ **Optional Updates** (Recommended)

If you have manual references to "SlashSense" in your own files:

1. **Update `~/.claude/CLAUDE.md` (if you added SlashSense references)**:

   ```diff
   - /ss:research - Fast research with 3 parallel agents
   + /ctx:research - Fast research with 3 parallel agents

   - "research best React state library" → /ss:research
   + "research best React state library" → /ctx:research
   ```

2. **Update project documentation** (if you documented SlashSense):

   ```diff
   - We use SlashSense for parallel development
   + We use Promptune for parallel development

   - Install: /plugin install slashsense
   + Install: /plugin install promptune
   ```

3. **Update any custom scripts** (if you call commands directly):
   ```diff
   - /ss:plan
   + /ctx:plan
   ```

---

## 🔄 Command Migration Map

All commands have been renamed with the new `/ctx:` prefix:

| Old Command     | New Command      | Status                       |
| --------------- | ---------------- | ---------------------------- |
| `/ss:research`  | `/ctx:research`  | ✅ Old command auto-detected |
| `/ss:plan`      | `/ctx:plan`      | ✅ Old command auto-detected |
| `/ss:execute`   | `/ctx:execute`   | ✅ Old command auto-detected |
| `/ss:status`    | `/ctx:status`    | ✅ Old command auto-detected |
| `/ss:cleanup`   | `/ctx:cleanup`   | ✅ Old command auto-detected |
| `/ss:configure` | `/ctx:configure` | ✅ Old command auto-detected |
| `/ss:stats`     | `/ctx:stats`     | ✅ Old command auto-detected |

**Note:** You can use either the old or new command names. Both work!

---

## 💬 Natural Language (Unchanged)

Natural language triggers work exactly as before:

```
✅ "research best React state library" → /ctx:research
✅ "create parallel plan for auth, dashboard, API" → /ctx:plan
✅ "what can Promptune do?" → skill: intent-recognition
```

**No changes needed!** Just keep talking naturally.

---

## 📦 Plugin Installation

### New Installation (v0.5.4+)

```bash
# Install Promptune
/plugin install promptune

# Verify installation
/plugin list
# Should show: promptune@0.5.4
```

### Upgrading from SlashSense

**Option 1: Automatic Update** (Recommended)

If you installed via marketplace, the update should happen automatically:

```bash
# Check current version
/plugin list

# If still showing slashsense@0.5.x, uninstall and reinstall:
/plugin uninstall slashsense
/plugin install promptune
```

**Option 2: Manual Update** (Local development)

If you're developing locally:

```bash
# Navigate to your local clone
cd ~/path/to/slashsense  # Your old repo

# Pull latest changes
git fetch origin
git checkout main
git pull origin main

# Or clone fresh
cd ~/Projects
git clone https://github.com/promptune/promptune
cd promptune

# Install locally
/plugin install promptune@local
```

---

## 🔍 Verification

After upgrading, verify everything works:

### 1. Check Plugin Version

```bash
/plugin list
```

**Expected output:**

```
✅ promptune@0.5.4 - Precision-tuned context engineering
```

### 2. Test Command

Try any command:

```bash
/ctx:research best state management library
```

**Expected:** Research agents spawn and return results

### 3. Test Natural Language

Just type naturally:

```
create parallel plan for user auth and dashboard
```

**Expected:** Claude detects intent and runs `/ctx:plan`

### 4. Check Session Start Message

Start a new session (or type `/clear`):

**Expected output:**

```
💡 Promptune Active (v0.5.4)

Quick Commands:
  /ctx:research - Fast research with 3 parallel agents (1-2 min, ~$0.07)
  /ctx:plan - Create parallel development plan
  ...
```

---

## ❓ FAQ

### Q: Will my old `/ss:*` commands stop working?

**A:** No! Old commands are automatically detected via natural language processing. Both `/ss:plan` and `/ctx:plan` work.

### Q: Do I need to update my workflows?

**A:** No, unless you have hardcoded references. Natural language triggers work exactly as before.

### Q: What if I have `/ss:*` in my CLAUDE.md?

**A:** It will still work, but we recommend updating to `/ctx:*` for clarity and future-proofing.

### Q: Why the rebrand?

**A:** "SlashSense" emphasized slash command detection, but the real value is **context engineering**:

- Modular plans (95% fewer tokens)
- Parallel workflows (81% cost reduction)
- Zero-transformation architecture
- Intelligent context flow optimization

"Promptune" better represents these core capabilities.

### Q: Is this a breaking change?

**A:** No! Version 0.5.4 is just a rebrand. All functionality is preserved and we're still in experimental 0.x status.

### Q: Do I lose my custom configurations?

**A:** There are no custom configurations to lose! Promptune v0.5.4 works out-of-the-box with zero configuration.

### Q: What about the GitHub repository?

**A:** The repository has been renamed:

- Old: `github.com/promptunecc/slashsense`
- New: `github.com/promptunecc/promptune`

GitHub automatically redirects old URLs to the new repo.

---

## 🆘 Troubleshooting

### Issue: "Plugin not found: promptune"

**Solution:**

```bash
# Check if marketplace is up to date
/plugin update

# If still not found, install from GitHub
/plugin install https://github.com/promptune/promptune
```

### Issue: "Old slashsense still installed"

**Solution:**

```bash
# Uninstall old version
/plugin uninstall slashsense

# Install new version
/plugin install promptune

# Verify
/plugin list
```

### Issue: "Commands not being detected"

**Solution:**

```bash
# Restart Claude Code session
# (Commands should appear in SessionStart hook)

# Or clear and start fresh
/clear

# Test detection
/ctx:research test
```

### Issue: "Getting errors with /ss: commands"

**Solution:** Update to new `/ctx:` prefix:

```bash
# Old (still works but deprecated)
/ss:plan

# New (recommended)
/ctx:plan
```

---

## 📚 Additional Resources

- [Promptune README](./README.md) - Full documentation
- [CHANGELOG](./CHANGELOG.md) - Detailed version history
- [CLAUDE.md](./CLAUDE.md) - Developer guide
- [GitHub Discussions](https://github.com/promptune/promptune) - Community support

---

## 💡 What's Next?

With the rebrand complete, we're doubling down on context engineering:

### Coming Soon:

- Enhanced modular plan templates
- Context flow visualization
- Real-time token usage tracking
- Performance benchmarking tools
- Multi-project context sharing

### Long-term Vision:

- **Context-aware development** - Claude Code that understands your entire workflow
- **Automatic optimization** - Zero-config context tuning
- **Team collaboration** - Shared context across team members
- **Context persistence** - Long-term memory across sessions

---

**Welcome to Promptune!** 🎵

Questions? Open an issue on [GitHub](https://github.com/promptune/promptune) or check [Discussions](https://github.com/promptune/promptune).
