<p align="center">
    <img src="assets/logo.svg" alt="Codex Git - Agent Skill" height="100" />
</p>

# Codex Git

This repository contains the `codex-git` agent skill.

## Purpose

Use this skill when working with git in Codex.app. It explains when git commands that write repository state require escalation, how to avoid conflicting write attempts, and how to interpret `.git/index.lock` failures conservatively.

## Compatibility

- Agent hosts: Codex.app and compatible Codex-style hosts
- Publication class: `portable-with-prereqs`

## Prerequisites

A Codex-style host with command escalation support

## Shared Baseline

This skill does not require the shared agents baseline.

## Contents

- `SKILL.md`
- optional support folders such as `references/`, `assets/`, or `scripts/`
