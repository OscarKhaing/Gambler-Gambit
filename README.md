# Milestone 2
---
### Q1\) Explain what your AI agent does in terms of PEAS. What is the "world" like? 
Our agent simulates a Vegas Triple Attack Blackjack Switch (VTABS) player with the goal of maximizing money won in the game while minimizing busting. We innovate on the regular game of Blackjack by combining it with **Triple Attack** and **Blackjack Switch**. By leveraging strategic decision-making, our model aims to improve blackjack play by balancing risk and reward while operating under uncertainty. Its success is measured through statistical performance metrics that assess its efficiency and effectiveness in achieving optimal outcomes.
#### Features of Triple Attack
- Players may make three bets: 
  - One initial bet (First Attack)
  - Another bet after the first card is dealt (Second Attack)
  - And a third and final bet after the second card is dealt (Third Attack).
#### Features of Blackjack Switch
- Players are dealt two hands (normal Blackjack uses one hand: a starting set of 2 cards)
- After seeing their cards, players can swap the second card of each hand to optimize their chances of winning
Our agent doesn't necessarily compete against another player - instead, its performance is described below:
#### Performance Measure
Our agent's performance is based on several key metrics, including the average value of its hand, win-to-loss ratio in units of dollars against the dealer, probability of avoiding a bust, and the number of rounds won based on proximity to 21.
#### Environment
The blackjack environment in which our agent operates is partially observable, as the player has no knowledge of the deck's composition. It is also stochastic, given that drawing a card introduces randomness, and sequential, since each decision affects future actions. Additionally, the environment is static, meaning the rules remain unchanged, discrete, as there are limited actions, and single-agent, focusing solely on the player's decision-making process.
#### Actuators
Our agent can perform five distinct actions:
1. *"Hit"* - draw a card
2. *"Stand"* - end its turn
3. *"Split"* - when dealt two identical cards
4. *"Bet"* - once before the round starts, once more after they see their first card and a final time after they see the dealer’s first card
5. *"Swap"* - each player gets two hands and can swap the second card between them
In practice, this will be done by returning a string with the action it will take.
#### Sensors
The actions above are chosen based on information from our agent's sensors, which include only the cards in the player's hand and the dealer’s face up card. In practice, we will simulate this by passing in this information as parameters to our agent.

### Q2\) What kind of agent is it? Goal based? Utility based? etc.
We have a utility based agent. Its goal is to win as many games as possible to *maximize* its profit. To this end, it takes the information mentioned above as its parameters and, using the data we train it on, decides which course of action is most optimal to proceed towards this goal. Specifically, it analyzes traits we mentioned in the **Performance Measure** section and seeks to minimize or maximize them in order to achieve this goal. To elaborate:
- We want to increase average value of its hand, but not over 20 - in other words, *balance* the value of our hands
- We want to increase the win-to-loss ratio in units of dollars against the dealer
- We want to minimize probability of a bust
- We want to maximize number of rounds won based on proximity to 21

### Q3\) Describe how your agent is set up and where it fits in probabilistic modeling
We are unsure of what this question is asking for, so here is our abstract:
```
Our blackjack agent is designed to simulate the game from a player's perspective, optimizing decisions to maximize hand value while minimizing the risk of busting. Unlike traditional blackjack models, our agent does not compete against another player; instead, its performance is evaluated based on key metrics, including the average value of its hand, win-to-loss ratio against the dealer, probability of avoiding a bust, and the number of rounds won based on proximity to 21. The blackjack environment in which our agent operates is partially observable, as the player has no knowledge of the deck's composition. It is also stochastic, given that drawing a card introduces randomness, and sequential, since each decision affects future actions. Additionally, the environment is static, meaning the rules remain unchanged, discrete, as there are limited actions, and single-agent, focusing solely on the player's decision-making process. Our agent can perform three distinct actions: Hit (draw a card), Stand (end its turn) and Split (when dealt two identical cards). These actions are chosen based on information from its sensors, which include only the cards in the player's hand. By leveraging strategic decision-making, our model aims to improve blackjack play by balancing risk and reward while operating under uncertainty. Its success is measured through statistical performance metrics that assess its efficiency and effectiveness in achieving optimal outcomes.
```

### Q4\) Train your first model
`model.ipynb` contains training, evaluation

### Q5\) Evaluate your model
`model.ipynb` contains training, evaluation

### Q6\) Create/Update your README.md to include your new work and updates you have all added. Make sure to upload all code and notebooks. Provide links in your README.md
New work:
- [blkjckhands.csv](https://github.com/OscarKhaing/Gambler-Gambit/blob/Milestone2/blkjckhands.csv) = original datafile
- [preprocessed_blkjckhands.csv](https://github.com/OscarKhaing/Gambler-Gambit/blob/Milestone2/preprocessed_blkjckhands.csv) = preprocessed datafile
- probability_data.json & refined_probability_data.json = intermediary probability data tables derived from the dataset that the agent uses (just for storage, the model operations are not affected)
- [model.ipynb](https://github.com/OscarKhaing/Gambler-Gambit/blob/Milestone2/model.ipynb) = training and evaluation of model
- [Preprocessing.ipynb](https://github.com/OscarKhaing/Gambler-Gambit/blob/Milestone2/Preprocessing.ipynb) = data exploration and data preprocessing

### Q7\) Conclusion section: What is the conclusion of your 1st model? What can be done to possibly improve it?
Our first model has only the capability of making decisions to hit or not, so that is something to be implemented in the future with risk mechanism and amount decider. For what we have, we achieved `Simulation Results: {'win': 658, 'loss': 342}` so it definitely has significant performance.
