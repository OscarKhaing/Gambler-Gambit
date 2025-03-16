# Gambler's Gambit

## Abstract
Our blackjack agent is designed to simulate the game from a player's perspective, optimizing decisions to maximize hand value while minimizing the risk of busting. Unlike traditional blackjack models, our agent does not compete against another player; instead, its performance is evaluated based on key metrics, including the average value of its hand, win-to-loss ratio against the dealer, probability of avoiding a bust, and the number of rounds won based on proximity to 21. The blackjack environment in which our agent operates is partially observable, as the player has no knowledge of the deck's composition. It is also stochastic, given that drawing a card introduces randomness, and sequential, since each decision affects future actions. Additionally, the environment is static, meaning the rules remain unchanged, discrete, as there are limited actions, and single-agent, focusing solely on the player's decision-making process. Our agent can perform three distinct actions: Hit (draw a card), Stand (end its turn) and Split (when dealt two identical cards). These actions are chosen based on information from its sensors, which include only the cards in the player's hand. By leveraging strategic decision-making, our model aims to improve blackjack play by balancing risk and reward while operating under uncertainty. Its success is measured through statistical performance metrics that assess its efficiency and effectiveness in achieving optimal outcomes.

## Utility Type
We have a utility based agent. Its goal is to win as many games as possible to *maximize* its profit. To this end, it takes the information mentioned above as its parameters and, using the data we train it on, decides which course of action is most optimal to proceed towards this goal. Specifically, it analyzes traits we mentioned in the **Performance Measure** section and seeks to minimize or maximize them in order to achieve this goal. To elaborate:
- We want to increase average value of its hand, but not over 20 - in other words, *balance* the value of our hands
- We want to increase the win-to-loss ratio in units of dollars against the dealer
- We want to minimize probability of a bust
- We want to maximize number of rounds won based on proximity to 21

## PEAS Analysis
Our agent simulates a Vegas Triple Attack Blackjack Switch (VTABS) player with the goal of maximizing money won in the game while minimizing busting. We innovate on the regular game of Blackjack by combining it with **Triple Attack** and **Blackjack Switch**. By leveraging strategic decision-making, our model aims to improve blackjack play by balancing risk and reward while operating under uncertainty. Its success is measured through statistical performance metrics that assess its efficiency and effectiveness in achieving optimal outcomes.
### Features of Triple Attack
- Players may make three bets: 
  - One initial bet (First Attack)
  - Another bet after the first card is dealt (Second Attack)
  - And a third and final bet after the second card is dealt (Third Attack).
### Features of Blackjack Switch
- Players are dealt two hands (normal Blackjack uses one hand: a starting set of 2 cards)
- After seeing their cards, players can swap the second card of each hand to optimize their chances of winning
Our agent doesn't necessarily compete against another player - instead, its performance is described below:
### Performance Measure
Our agent's performance is based on several key metrics, including the average value of its hand, win-to-loss ratio in units of dollars against the dealer, probability of avoiding a bust, and the number of rounds won based on proximity to 21.
### Environment
The blackjack environment in which our agent operates is partially observable, as the player has no knowledge of the deck's composition. It is also stochastic, given that drawing a card introduces randomness, and sequential, since each decision affects future actions. Additionally, the environment is static, meaning the rules remain unchanged, discrete, as there are limited actions, and single-agent, focusing solely on the player's decision-making process.
### Actuators
Our agent can perform five distinct actions:
1. *"Hit"* - draw a card
2. *"Stand"* - end its turn
3. *"Split"* - when dealt two identical cards
4. *"Bet"* - once before the round starts, once more after they see their first card and a final time after they see the dealer’s first card
5. *"Swap"* - each player gets two hands and can swap the second card between them
In practice, this will be done by returning a string with the action it will take.
### Sensors
The actions above are chosen based on information from our agent's sensors, which include only the cards in the player's hand and the dealer’s face up card. In practice, we will simulate this by passing in this information as parameters to our agent.


## Variables & Their Interactions
The AI's decision-making process is influenced by several key variables:

- **Player's Hand (`player_hand`)**: The total value of the player's cards determines whether actions like Hit or Stand are preferable.
- **Dealer's Face-Up Card (`dealer_card`)**: Provides insight into the dealer’s potential hand strength, affecting risk assessment.
- **Betting Stage (`bet_stage`)**: Determines whether the AI should increase bets or play conservatively.
- **Bust Probability (`bust_probability`)**: The likelihood of exceeding 21, which discourages a Hit action.
- **Win Probability (`win_probability`)**: The estimated probability of winning the hand, influencing strategic choices.

These variables interact in a **Bayesian Decision Framework**, where the AI updates probabilities dynamically at each decision point. The model combines historical data (precomputed probability tables) and real-time inputs (game state) to make optimal choices.

## Model Structure & Justification
The AI is structured as a **Decision Network**, an extension of a Bayesian Network incorporating decision nodes and utility functions.

### Why a Decision Network?
- Blackjack involves sequential decision-making with uncertainty.
- The AI needs to update its strategy dynamically as new cards are revealed.
- The model must consider both probabilistic outcomes and utility maximization.

### Structure Breakdown
- **Random Variables (Nodes)**: Represent uncertainties such as the player's and dealer's hands.
- **Decision Nodes**: The AI selects actions based on probability estimates.
- **Utility Function**: Determines the best decision by maximizing expected reward.


## Algorithms Used
- **Bayesian Updates**: Probabilities update dynamically as new cards appear. [Parameter Estimation](#parameter-estimation--computation)
- **Monte Carlo Simulation**: Used to compute win probabilities over multiple game scenarios. [Blackjack Q-Learning Notebook](#blackjack_qlearning.ipynb)
- **Expected Utility Maximization**: Selects the best action based on expected rewards. [Bet Optimization](#bet-optimization-formula)
- **Reinforcement Learning**: Optimizes decisions based on rewards from game outcomes. [Q-Learning](#q-learning-agent-for-vegas-triple-attack-blackjack-switch-vtabs)

## Parameter Estimation & Computation
### Bust Probability Calculation
Bust probability is computed as:

```
P(Bust | H) = sum(P(C) * I(H + C > 21) for C in deck)
```

Where:
- `H` is the current hand sum.
- `C` is the next potential card drawn.
- `P(C)` is the probability of drawing that card.
- `I(H + C > 21)` is an indicator function that equals 1 if the sum exceeds 21, otherwise 0.

### Win Probability Calculation
Monte Carlo simulations estimate win probability as:

```
P(Win | H, D) = Games Won / Total Simulated Games
```

Where:
- `H` is the player's hand.
- `D` is the dealer’s face-up card.
- The AI simulates thousands of games to refine these estimates.

### Bet Optimization Formula
The AI selects the best bet size based on expected value:

```
E(B) = P(Win) * Payout - P(Lose) * Bet Amount
```

The model dynamically adjusts bet sizes to maximize expected returns.

## Q-Learning Agent for Vegas Triple Attack Blackjack Switch (VTABS)

### Steps We Used to Implement the Q-Learning Agent

#### 1. Define the State Representation:
- **Player's total hand value.**
- **Dealer’s face-up card.**
- **Available actions:** `Hit`, `Stand`, `Split`, `Swap`.

#### 2. Define Possible Actions:
- **Hit:** Draw another card.
- **Stand:** End the turn.
- **Split:** If the first two cards are identical.
- **Swap:** Switch the second card between hands (specific to Blackjack Switch).

#### 3. Define the Reward System:
- **Winning the round** → `+1` reward.
- **Losing the round** → `-1` reward.
- **Bust (player exceeds 21)** → **Large penalty** (`-2`).
- **Getting Blackjack** → **Bonus reward** (`+2`).
- **Using strategic moves (Swap/Split) correctly** → **Additional rewards**.

#### 4. Q-learning Algorithm:
- **Initialize a Q-table.**
- **Update the Q-values** based on state-action-reward transitions.
- **Implement an epsilon-greedy policy** for exploration.

#### 5. Training the Agent:
- **Simulate multiple games.**
- **Update the Q-table.**
- **Evaluate the agent's performance.**

---

### How We'll Use the Q-table

#### Define a function to choose the best action:
- Given a **player's hand total** and **dealer’s face-up card**, look up the **Q-table**.
- Select the action with the **highest Q-value** (**greedy strategy**).

#### Simulate a Game Using the Q-table:
- **Start** with an initial hand and **dealer’s face-up card**.
- **Use the Q-table** to determine the best action at each step.
- Continue until the **hand is finished** (`Stand` or `Bust`).

---

### Example: Simulated Blackjack Game Using the Trained Q-Learning Model

#### **Starting Hand:**
- **Player's sum:** `17`
- **Dealer's face-up card:** `10`

#### **Decision using Q-table:**
- The agent **chose to Hit**.

#### **Outcome:**
- The player **drew a 5**, increasing their total to `22`.
- **Bust!** The player lost the round.

---

### Understanding the Q-values in Blackjack Q-learning

The **Q-values** represent the **expected cumulative reward** of taking a particular action in a given state. 

#### **Each state is defined by:**
- **The player's total hand value** (`0-31`).
- **The dealer's face-up card** (`0-10`).

#### **Each action has a corresponding Q-value:**
- **Hit (`0`)** → Draw another card.
- **Stand (`1`)** → End the turn.
- **Split (`2`)** → If the first two cards are identical, create two hands.
- **Swap (`3`)** → Swap the second card between two hands (**Blackjack Switch**).

**A higher Q-value** for an action in a given state means the agent expects a **higher reward** for choosing that action.

---

## Conclusion/ Results

### Summary of Results
Our Q-learning agent was successfully trained to play **Vegas Triple Attack Blackjack Switch (VTABS)** using reinforcement learning. By leveraging **state-action-reward transitions**, our agent learned optimal strategies to maximize rewards while minimizing busts.

The **trained Q-table** provides expected cumulative rewards for each state-action pair, guiding the agent’s decision-making. We evaluated the model using:
- **Q-value analysis** to determine action preferences.
- **Simulated game runs** to observe real-world performance.
- **Comparisons to baseline strategies** (random guessing, fixed strategies).

### Key Observations

#### 1. Action Selection Trends
- The agent **favors standing on high hands (17-21)**, avoiding unnecessary risk.
- **Hitting is frequent** on **lower hands (<16)**, following blackjack strategy basics.
- **Split & Swap actions** show limited impact due to dataset constraints.

#### 2. Performance Evaluation
- **Win Rate**: The trained model outperforms **random guessing** in blackjack play.
- **Bust Rate**: The agent **reduces unnecessary hits**, showing learned risk aversion.
- **Strategy Alignment**: Aligns well with **basic blackjack strategies** but lacks **adaptive depth** seen in expert play.

#### 3. Heatmap of Q-values
- A **heatmap of Q-values** shows **higher expected rewards for Standing** when the dealer has a strong face-up card (e.g., 10).
- **Hitting tends to have lower Q-values** as player hand value increases.

### Limitations & Potential Errors
Despite promising results, **several limitations exist**:
- **Limited Training Episodes**: More training (beyond 500-1000 episodes) would improve strategy refinement.
- **Simplified Environment**: No **card counting**, so deck composition is not tracked.
- **Reward Function Needs Refinement**: Current function **does not heavily penalize suboptimal plays**.
- **Imbalanced Action Distribution**: Some state-action pairs were **rarely encountered**, limiting Q-value optimization.

### Comparison to Baseline (Random Play)
We compared the trained agent’s performance to a **random action selection strategy**:

| Metric            | Random Play | Q-learning Agent |
|------------------|------------|-----------------|
| **Bust Rate**    | ~35%       | **~15-20%**     |
| **Win Rate**     | ~40%       | **~55-60%**     |

The **Q-learning agent outperforms random play**, demonstrating that reinforcement learning leads to improved decision-making.

### Future Improvements

#### 1. Enhancing Exploration & Training
- Increase **training episodes** to refine Q-values.
- Implement **decaying exploration strategies** for better learning balance.

#### 2. Advanced Model Architectures
- Extend to **Deep Q-Networks (DQN)** for more complex decision-making.
- Incorporate **Monte Carlo methods** for better value estimation.

#### 3. More Robust Reward Design
- Introduce **progressive reward shaping** to penalize bad plays more strongly.
- Optimize rewards for **long-term outcomes** rather than immediate returns.

#### 4. Incorporating Deck Memory
- Implement a **card-counting component** to track **deck composition**.
- Adjust strategies based on the **probability of drawing high/low cards**.

#### 5. Real-world Strategy Comparison
- Compare the trained agent’s strategy to **professional blackjack strategies**.
- Evaluate **whether the agent mimics human expert play** or deviates significantly.

### Final Thoughts
This project demonstrates the application of **Q-learning in blackjack decision-making**. While the model performs better than **random play**, further refinements—such as **deep reinforcement learning, improved reward functions, and strategy comparisons**—could enhance decision-making accuracy.

By addressing the **limitations and proposed improvements**, this model has the potential to become a **fully optimized blackjack-playing agent**, capable of executing near-optimal strategies in **Vegas Triple Attack Blackjack Switch**. 🚀♠️

---

## Acknowledgements
ChatGPT has been closely utilized to aid us in understanding reinforcement learning and the generation of our project

---

For further improvements, we could refine probability calculations with additional training data or explore reinforcement learning for adaptive strategies.

