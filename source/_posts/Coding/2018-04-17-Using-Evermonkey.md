---
date: 2018-04-17 00:00:00
title: "Using Evermonkey"
category: "Coding"
tag: ["evermonkey", "evernote"]
---

# Using Evermonkey #

I've been using VSCode for a while and used to Markdown, which has not been supported by EverNote yet. Thus, I wonder whether there's any extension that can help. Luckily, [evermonkey](https://github.com/michalyao/evermonkey) shows up.

## Installation ##

There are 3 steps to use this extension.

1. Get a developer token. Currently, EverNote does not accept applications for tokens on their official website. But we can get a token by sending emails to their costumer service. I got a token in only one or two days.
2. Install evermonkey extension to VSCODE.
3. Set `evermonkey.token` and `evermonkey.noteStoreUrl` in settings.

## Usage ##

Open command panel by F1 or ctrl+shift+p then type

* `ever new` to start a new blank note.
* `ever open` to open a note in a tree-like structure.
* `ever search` to search note in EverNote grammar.
* `ever publish` to publish current editing note to EverNote server.
* `ever sync` to synchronizing EverNote account.

## Shortage ##

Currently, third-party extensions only support synchronizing files. The file can not be modified in apps. For example, I can now only modify the file in VSCODE, but not in EverNote application.