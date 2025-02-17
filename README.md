### Explain what your AI agent does in terms of PEAS. What is the "world" like? 
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

### What kind of agent is it? Goal based? Utility based? etc.
<>(TODO)

### Describe how your agent is set up and where it fits in probabilistic modeling
<>(TODO)

### Train your first model
<>(TODO)

### Evaluate your model
<>(TODO)

### Create/Update your README.md to include your new work and updates you have all added. Make sure to upload all code and notebooks. Provide links in your README.md
<>(TODO)

### Conclusion section: What is the conclusion of your 1st model? What can be done to possibly improve it?
<>(TODO)