# Adversary review pattern for Claude Code

This pattern runs a second Claude Code session as a dedicated adversarial reviewer. One session does the work. A separate "adversary" session finds the flaws. The two sessions talk over a socket.

CLAUDE.md contains the instructions for the adversary role.

## Setup

1. ```git clone https://github.com/blueben/adversary.git```
2. ```cd adversary```
3. Run ```claude -n adversary```
4. Accept the Trust prompt
5. Select the model you wish to use.
6. Let the session idle

## Use

From any other Claude Code session, say: "Send this plan to the adversary session for review." The adversary session reviews the submission and returns feedback to the session that asked.

## Model pairing

Run the worker session on a cheaper model (Sonnet). Run the adversary session on a pricier model (Opus or Fable). The cheap model does the work. The expensive model does the review.

## Highly suggested

Install the Plain English skill, which enables Adversary to provide less ambiguous and more precise feedback.
https://github.com/b1rdmania/claude-plain-english-skill

## Safety

#### Default config (.claude/settings.json)

- The session starts in plan mode.
- Write, Edit, and NotebookEdit are denied outright, blocking file modification through Claude Code's own tools regardless of mode.
- The sandbox is enabled and set to fail closed.
- The sandbox denies filesystem writes across the entire root at the OS level.
- Auto memory is off: nothing about the session persists across runs, so each one starts with a clean context.

#### Optional restrictions (.claude/settings.restricted.json)

This additional configuration further restrict's claude's capabilities, at the cost of potentially useful functionality. If you need some tools for review purposes, use this configuration as a template and allow your own MCPs, allowed commands, and network access.

To use this, run ```claude -n adversary --settings .claude/settings.restricted.json```

- Bash is denied.
- WebFetch and WebSearch are denied.
- No MCP server loads, from any source.
- The sandbox's escape hatch for retrying failed commands outside the sandbox is disabled.
- Network access for sandboxed commands is locked to an empty allowlist with strict enforcement.
- Messages from sessions on other machines require explicit approval.
