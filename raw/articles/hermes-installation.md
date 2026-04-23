---
source_url: https://hermes-agent.nousresearch.com/docs/getting-started/installation
ingested: 2026-04-23
sha256: 118c2478c0fd9fed58937e056dcc56ce29451916582242199347f1b04f7b786c
---
Installation | Hermes Agent

Skip to main content

On this page

Get Hermes Agent up and running in under two minutes with the one-line installer.

Quick Install

​

Linux / macOS / WSL2

​

curl

-fsSL

https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh

|

bash

Android / Termux

​

Hermes now ships a Termux-aware installer path too:

curl

-fsSL

https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh

|

bash

The installer detects Termux automatically and switches to a tested Android flow:

uses Termux

pkg

for system dependencies (

git

,

python

,

nodejs

,

ripgrep

,

ffmpeg

, build tools)

creates the virtualenv with

python -m venv

exports

ANDROID_API_LEVEL

automatically for Android wheel builds

installs a curated

.[termux]

extra with

pip

skips the untested browser / WhatsApp bootstrap by default

If you want the fully explicit path, follow the dedicated

Termux guide

.

Windows

Native Windows is

not supported

. Please install

WSL2

and run Hermes Agent from there. The install command above works inside WSL2.

What the Installer Does

​

The installer handles everything automatically — all dependencies (Python, Node.js, ripgrep, ffmpeg), the repo clone, virtual environment, global

hermes

command setup, and LLM provider configuration. By the end, you're ready to chat.

After Installation

​

Reload your shell and start chatting:

source

~/.bashrc

# or: source ~/.zshrc

hermes

# Start chatting!

To reconfigure individual settings later, use the dedicated commands:

hermes model

# Choose your LLM provider and model

hermes tools

# Configure which tools are enabled

hermes gateway setup

# Set up messaging platforms

hermes config

set

# Set individual config values

hermes setup

# Or run the full setup wizard to configure everything at once

Prerequisites

​

The only prerequisite is

Git

. The installer automatically handles everything else:

uv

(fast Python package manager)

Python 3.11

(via uv, no sudo needed)

Node.js v22

(for browser automation and WhatsApp bridge)

ripgrep

(fast file search)

ffmpeg

(audio format conversion for TTS)

info

You do

not

need to install Python, Node.js, ripgrep, or ffmpeg manually. The installer detects what's missing and installs it for you. Just make sure

git

is available (

git --version

).

Nix users

If you use Nix (on NixOS, macOS, or Linux), there's a dedicated setup path with a Nix flake, declarative NixOS module, and optional container mode. See the

Nix & NixOS Setup

guide.

Manual / Developer Installation

​

If you want to clone the repo and install from source — for contributing, running from a specific branch, or having full control over the virtual environment — see the

Development Setup

section in the Contributing guide.

Troubleshooting

​

Problem

Solution

hermes: command not found

Reload your shell (

source ~/.bashrc

) or check PATH

API key not set

Run

hermes model

to configure your provider, or

hermes config set OPENROUTER_API_KEY your_key

Missing config after update

Run

hermes config check

then

hermes config migrate

For more diagnostics, run

hermes doctor

— it will tell you exactly what's missing and how to fix it.

Quick Install

Linux / macOS / WSL2

Android / Termux

What the Installer Does

After Installation

Prerequisites

Manual / Developer Installation

Troubleshooting