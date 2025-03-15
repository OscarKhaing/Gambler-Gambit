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

## Algorithms Used
- **Bayesian Updates**: Probabilities update dynamically as new cards appear.
- **Monte Carlo Simulation**: Used to compute win probabilities over multiple game scenarios.
- **Expected Utility Maximization**: Selects the best action based on expected rewards.

## Conclusion
This Blackjack AI employs a **Decision Network** structure to model uncertainty and optimize decision-making. By integrating **probability calculations, simulations, and utility-based decisions**, the AI adapts dynamically to the game state, improving its ability to make strategic choices in a probabilistic environment.

---

For further improvements, we could refine probability calculations with additional training data or explore reinforcement learning for adaptive strategies.

