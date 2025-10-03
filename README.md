# Tmux ML Training Runner

A simple, flexible script to run ML training (or any command) in tmux with automatic logging, monitoring windows, and environment activation.

## Features

- Run any command in a tmux session
- Automatic timestamped output directories
- Full logging with `tee` to capture all output
- Multiple monitoring windows (GPU, logs, system)
- Environment activation (conda, venv, or custom)
- Non-interactive by default
- Can run without tmux for debugging

## Quick Start

### Basic Usage
```bash
bash run_ml_workflow.sh \
  --cmd "python train.py" \
  --session training
```

### With Conda Environment
```bash
bash run_ml_workflow.sh \
  --cmd "python train.py --epochs 100" \
  --env-type conda --env-name myenv \
  --session training \
  --tag experiment1 \
  --attach
```

### With Config and Resume
```bash
bash run_ml_workflow.sh \
  --cmd "python scripts/train.py" \
  --env-type conda --env-name ml-env \
  --config configs/model.yaml \
  --resume checkpoints/last.ckpt \
  --session training --tag v2 \
  --force --attach
```

## Output Structure

Each run creates a timestamped directory:

```
outputs/run_20251003_200748/
├── checkpoints/        # For model checkpoints
├── logs/              # Training logs
│   └── training_20251003_200748.log
├── results/           # For metrics/results
└── session_info.sh    # Session information script
```

