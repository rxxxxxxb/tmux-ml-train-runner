# tmux-workflow-runner

A robust, general-purpose **ML workflow runner** that uses [tmux](https://github.com/tmux/tmux) to manage training, experiments, or any long-running commands.  
It automatically handles **logging, monitoring, environment activation, and reproducible output directories**.

## Features

- Run **any command** (training, evaluation, preprocessing, testing, etc.)
- Automatic **output directory structure** with timestamp + optional tags
- **tmux integration**: multiple windows for training, monitoring, logs, GPU usage, and system resources
- **Full logging** with `tee`, stored under `outputs/<timestamp>/logs/`
- Non-interactive by default (use `--force` to replace sessions, `--attach` to auto-attach)
- Optional monitoring commands (custom or built-in)


