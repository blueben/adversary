# Adversary review pattern for Claude Code

This pattern runs a second Claude Code session as a dedicated adversarial reviewer. One session does the work. A separate "adversary" session finds the flaws. The two sessions talk over a socket.

CLAUDE.md contains the instructions for the adversary role.

## Setup

1. ```git clone https://github.com/blueben/adversary.git```
2. ```cd adversary```
3. Run ```claude --settings adversary-settings.json```
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
