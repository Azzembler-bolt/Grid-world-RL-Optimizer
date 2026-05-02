
# 3D Gridworld — MDP & Reinforcement Learning

A tabular Q-learning agent trained to navigate a stochastic 3D gridworld, formulated as a Markov Decision Process (MDP). The agent learns to reach a goal state while avoiding a pit and obstacles, with support for slippery (stochastic) transitions.

---

## Problem Overview

The environment is a **6×6×6 3D grid** where an agent must find an optimal path from a random start position to the goal at `(5, 5, 5)`, while avoiding the pit at `(2, 2, 2)` and several fixed obstacles scattered across the grid.

### MDP Formulation

| Component | Description |
|---|---|
| **State Space** | All valid `(x, y, z)` positions in a 6×6×6 grid (excluding obstacles) |
| **Actions** | 6 directions — East, West, North, South, Up, Down |
| **Goal Reward** | +50 at `(5, 5, 5)` |
| **Pit Penalty** | −50 at `(2, 2, 2)` |
| **Step Cost** | −1 for every other valid move |
| **Discount Factor (γ)** | 0.95 |
| **Slip Probability** | 0.2 (agent moves in a perpendicular direction with probability 0.2) |

---

## Project Structure

```
├── Gridworld_Assignment.ipynb   # Main notebook (environment, training, evaluation, visualisation)
├── requirements.txt             # Python dependencies
└── README.md
```

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the notebook

```bash
jupyter notebook Gridworld_Assignment.ipynb
```

---

## Implementation Details

### Environment — `Gridworld3D`

- Stochastic transitions: with probability `slip_prob` (default 0.2), the agent slips into a random perpendicular direction instead of the intended one.
- If a move leads out of bounds or into an obstacle, the agent stays in place.
- The episode ends when the agent reaches the goal or falls into the pit.
- The start position is randomised each episode.

### Agent — `QLearningAgent`

Tabular Q-learning with ε-greedy exploration.

| Hyperparameter | Value |
|---|---|
| Learning rate (α) | 0.1 |
| Discount factor (γ) | 0.95 |
| Initial ε | 1.0 |
| ε decay | 0.9999 per episode |
| Minimum ε | 0.01 |
| Training episodes | 15,000 |

The Q-table is a `6×6×6×6` numpy array (states × actions).

---

## Results

After training for 15,000 episodes:

| Policy | Average Return (100 episodes) |
|---|---|
| **Learned (greedy)** | **40.80** |
| Random baseline | −204.20 |

The learned policy significantly outperforms random wandering, consistently reaching the goal while avoiding the pit.

---

## Visualisation

The notebook generates:

- **Learning curve** — total reward per episode with a 100-episode moving average, showing convergence over training.
- **Policy heatmaps** — 2D slices of the 3D grid at z-levels 2, 3, and 4, showing:
  - Cell colour = learned state value (`max_a Q(s, a)`)
  - Red arrows = greedy action at each state
  - Black cells = obstacles, `G` = goal, `P` = pit

---

## Experiments

The notebook explores the effect of varying:

- **Discount factor (γ):** Lower γ (e.g. 0.8) makes the agent short-sighted and risk-prone; higher γ (e.g. 0.99) leads to more globally optimal, cautious paths.
- **Slip probability:** Low slip (0.05) yields a fast, direct policy; high slip (0.5) forces the agent to learn conservative routes that steer clear of the pit and obstacles.

---

## Authors

- Yamsani Hari Charan — 2023A7PS0042P  
- Shikhar Agarwal — 2023A3PS0528P  
- Sarvesh Puram — 2023A2PS0231P  

*BITS Pilani | Due: 03/10/2026*
