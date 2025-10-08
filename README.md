# Tmux ML Training Runner

A simple script to run ML training (or any command) in tmux with automatic logging and monitoring windows.

## Why?

When you have a long-running training script like `train.py`, you want:
- **Persistent session** - survives disconnections
- **Automatic logging** - captures all output
- **Easy monitoring** - GPU, logs, system resources in separate windows

## Quick Start

### Basic Usage
```bash
bash run_ml_workflow.sh --cmd "python train.py"
```

### With Conda Environment
```bash
bash run_ml_workflow.sh \
  --cmd "python train.py --epochs 100" \
  --env-type conda --env-name myenv \
  --session my_training \
  --attach
```

### Testing
```bash
bash run_ml_workflow.sh \
  --cmd "python demo_train.py --epochs 3" \
  --session demo \
  --attach
```

## What It Does

1. Creates a tmux session with multiple windows:
   - **Training**: Your command runs here
   - **Logs**: Live tail of the log file
   - **GPU**: `nvidia-smi` (if available)
   - **System**: `htop` or `top`

2. Logs everything to `logs/training_TIMESTAMP.log`

3. Activates your conda/venv if specified

## Options

```
--cmd "COMMAND"          Command to run (required)
--session NAME           Tmux session name (default: training)
--log-dir PATH           Log directory (default: logs)
--attach                 Auto-attach to tmux after creation
--force                  Kill existing session if exists
--no-tmux                Run without tmux (for debugging)

--env-type TYPE          Environment: none|conda|venv
--env-name NAME          Conda env name or venv path
```

## Examples

### Simple Training
```bash
bash run_ml_workflow.sh \
  --cmd "python train.py --lr 0.001 --batch-size 32"
```

### With Environment
```bash
bash run_ml_workflow.sh \
  --cmd "python train.py --config config.yaml" \
  --env-type conda --env-name pytorch \
  --session training_run
```

### Debug Mode (No Tmux)
```bash
bash run_ml_workflow.sh \
  --cmd "python train.py" \
  --no-tmux
```

### Force Replace Existing Session
```bash
bash run_ml_workflow.sh \
  --cmd "python train.py" \
  --session training \
  --force --attach
```

## Tmux Commands

Once attached to the session:
- **Detach**: `Ctrl-b d` (session keeps running)
- **Switch windows**: `Ctrl-b 0` (training), `Ctrl-b 1` (logs), etc.
- **List windows**: `Ctrl-b w`

From terminal:
- **Attach**: `tmux attach -t SESSION_NAME`
- **Kill**: `tmux kill-session -t SESSION_NAME`
- **List sessions**: `tmux list-sessions`

## Output

All output is logged to:
```
logs/training_TIMESTAMP.log
```

Simple, timestamped, and easy to find.

## Requirements

- **tmux** (optional, but recommended)
  - macOS: `brew install tmux`
  - Ubuntu: `apt install tmux`
- **conda** or **venv** (optional, for environment activation)
- **nvidia-smi** (optional, for GPU monitoring)
- **htop** (optional, falls back to `top`)


## Essential Tmux Commands

### Basic Navigation

**While INSIDE the tmux session:**

| Action | Command |
|--------|---------|
| **Detach** (keep training running) | `Ctrl+B` then `D` |
| **Switch to window 0** (Training) | `Ctrl+B` then `0` |
| **Switch to window 1** (GPU Monitor) | `Ctrl+B` then `1` |
| **Switch to window 2** (System) | `Ctrl+B` then `2` |
| **Next window** | `Ctrl+B` then `N` |
| **Previous window** | `Ctrl+B` then `P` |
| **List windows** | `Ctrl+B` then `W` |


## Troubleshooting

**Session already exists?**
```bash
# Option 1: Use --force
bash run_ml_workflow.sh --cmd "..." --force

# Option 2: Attach to existing
tmux attach -t training

# Option 3: Use different name
bash run_ml_workflow.sh --cmd "..." --session other_name
```

**Tmux not available?**
```bash
# Run without tmux
bash run_ml_workflow.sh --cmd "..." --no-tmux
```

## License

MIT License
