# Kanso 簡素

**Efficient mode for Claude Code.**

*Kanso* is a Japanese design principle meaning "simplicity": remove the clutter,
keep only what's needed. This plugin applies that to code and to Claude's replies.

## What it does

Kanso makes Claude Code solve the whole problem with the least new code:

- **Stays in scope.** Questions get answers, not code. Only what the prompt
  asks gets done; small prompts get quick replies.
- **Smallest complete change.** Before writing anything, Claude checks whether
  the code needs to exist, then reuses what's already in the codebase, the
  standard library, or installed dependencies, and only then writes new code.
- **Matches your project.** New code follows the existing style, naming,
  patterns, and structure.
- **No overengineering.** No abstractions, wrappers, options, or "for later"
  code nobody asked for.
- **Complete, not just short.** Every caller, test, and fixture the change
  affects gets updated.
- **Short replies.** Usually 1-3 lines: the outcome, then what you must know
  (assumptions, risks, skipped features). Minimal text while it works.
- **Guardrails stay.** Security, validation at trust boundaries, error handling
  that prevents data loss, and accessibility are never cut.

The goal is smaller diffs, without stripping the checks that keep quality up.

## Installation

From inside Claude Code:

```
/plugin marketplace add erickjs11/kanso
/plugin install kanso@kanso
```

Then restart Claude Code or start a new session.

### Try it locally first

```
claude --plugin-dir ./kanso
```

## Turning it on and off

```
/kanso off      # stop following the rules now and in new sessions
/kanso on       # turn it back on
/kanso status   # show whether it's on
```

Claude Code prefixes plugin commands with the plugin name, so the full form is
`/kanso:kanso on`. Typing `/kanso` and picking it from the menu also works.

The setting lasts across sessions. It's stored as a `~/.kanso-off` file:
Kanso is off while that file exists.

## How it works

```
kanso/
├── .claude-plugin/
│   ├── plugin.json        # plugin manifest
│   └── marketplace.json   # lets the repo be installed as a marketplace
├── commands/
│   └── kanso.md           # /kanso on|off|status
├── hooks/
│   └── hooks.json         # SessionStart hook that loads the rules
├── scripts/
│   └── kanso-switch.sh    # flips the on/off setting
├── kanso.md               # the rules themselves
├── LICENSE
└── README.md
```

A `SessionStart` hook loads `kanso.md` into Claude's context at the start of
every session, including after `/clear` and context compaction, so the rules
stay active for the whole session. When Kanso is off, the hook loads nothing.

## Customizing

All the behavior lives in `kanso.md`. Edit it to fit your team.

## Uninstall

```
/plugin uninstall kanso@kanso
```

If you turned Kanso off, also delete `~/.kanso-off`.

## Credits

Kanso is based on [**Ponytail**](https://github.com/DietrichGebert/ponytail)
by [DietrichGebert](https://github.com/DietrichGebert), a "lazy senior dev"
plugin built on the idea that the best code is the code never written. Kanso
keeps two of its ideas: the ordered ladder for choosing the smallest solution
(shortened from seven rungs to five) and the "never cut" list. Kudos to its
author.

Beyond that, Kanso goes its own way:

- uses an "efficient" persona instead of "lazy," to keep the focus on doing
  the job well with less;
- puts matching the project's style at the core;
- defines a short, fixed reply format instead of detailed explanations;
- has a single on/off switch, with no intensity levels or review, audit, and
  debt commands;
- drops instructions Claude Code already follows by default.

## License

MIT
