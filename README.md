# Adversarial Reinforcement Learning for Portfolio Management
- A final project for E6885 _Reinforcement Learning_, Fall 2018, Columbia University.

- The full version of the final report [here](./RL_Final_Report_v2.pdf)

## Abstract
In this paper, we created a *virtual stock exchange* where multiple agents invest in stocks against each other. Three of the agents implemented their own strategies based on different Reinforcement Learning (RL) algorithms. *No historical data was used* to train these RL agents. RL agents are given cash only and do not have any stocks at an initialization phase. Trading environments were designed in a way they accommodate realistic factors such as liquidity costs (buy expensive, sell cheap due to little orders that can be matched), transaction costs, and a management fee. We achieved this by introducing a sophisticated framework that admits the complex nature of the trading environment in the real world.

We present an approach of which algorithms the RL agents are based on, how we set our environment, and how a virtual stock exchange was constructed. We used Deep Neural Network(DNN) to represent the Actor-Critic network, which takes an input of stock prices and portfolio weights and outputs a vector of portfolio weights. RL agents’ actions are concretely implemented by placing orders and they do not know how their states would be until the orders are matched. We introduced non-RL agents, one of which, namely a mean-reversion agent, is in favor of our market environment in its strategy, and the other of which, namely a random agent, is to model the irrational behaviors of the real stock market. We end this paper with experimental analysis and future work.


## 1. Introduction
The total value of stocks traded worldwide in the year 2017 was US$ 77 trillion which comprised of 117% of the world GDP for that year (WorldBank, 2018). Given that approximately 20% of households’ and nonprofit organizations’ total financial assets are estimated to be invested in equity markets (FRED, 2018), even a slight increase in percentage return would have far reaching effects in practice. The current distribution of the class of machine learning (ML) algorithms applied to finance is skewed towards supervised learning. Due to the nature of this relatively classical ML technique, most of its applications are limited to predicting stock prices based on historical data (Fischer, 2017). However, institutional- level investors require a more sophisticated and flexible approach to incorporate both the indigenous as well as the exogenous factors within a given quantitative investing strategy. Research centered around supervised learning has revealed a few major limitations of not incorporating such factors within the models. For example, the dynamics of transaction costs, which is an indigenous factor, is often not incorporated in modeling whereas in the real world, dedicated professionals are made responsible for reducing transaction costs in practice due to their large effect on performance. An exogenous factor often ignored is non-stationarity, i.e. making the investment strategies online - adapting to the changing market conditions.


In this project, we plan to create a virtual stock market wherein agents invest in stocks using their own Rein- forcement Learning (RL) and non-RL based strategies. In order to resolve the limitations of existing quantitative approaches, our project structurally accommodates as many realistic factors as possible by leveraging the power of RL. Furthermore, it is desirable to have an end to end integrated framework for training and back-testing the RL based systematic trading strategies prior to deploying it into production, which we attempt to provide.


The markets are constantly changing and often, the market regime may change without any pre-signal which severely hurts the values of investors’ assets. In history, capital markets have witnessed almost no single investor who has been able to beat the market consistently. Only a few famous investors such as Warren Buffet are known for their insightful investing, having overcome the negative effects of regime changes over a long period of time based on their qualitative value-investing philosophy.


One contrasting approach would be a quantitative invest- ing strategy which does not allow human intervention dur- ing any investment decision-making process. The strengths of traditional quantitative strategies have made them grow drastically in quality and quantity, leading to them being one of the two major strategies since time being. Currently, quantitative funds are responsible for approximately 25% of the total trades in the US (Zuckerman, 2017). Taking this further, we try to introduce a novel quantitative invest- ment model by introducing multi-agents working with dif- ferent kinds of trading strategies, imposing a realistic en- vironment with real-world trading rules, and performing industry-level evaluations toward the agents’ behavior. In addition, we attempt to learn from scratch using self-play, i.e. we do not use any historical market data for training our RL agents.


## 2. Background/Related Work
Surprisingly, RL based investing has drawn relatively less attention from researchers and practitioners as an alternative. (Liang, 2014) explores Deep Reinforcement Learning (DRL) in the context of portfolio management wherein the authors explore the influence of different optimizers and network structures on performance of trad- ing agents, utilizing two kinds of DRL algorithms, deep deterministic policy gradient (DDPG) and proximal policy optimization (PPO). One of their significant contribution is to show that the existence of transaction cost translates the problem from a prediction problem (whose global optimized policy can be obtained via a greedy algorithm) into a compute-expensive dynamic programming problem.


DRL techniques are proven to have their own advantages over classical RL. They approximate strategy or value functions using non-linear approximations (which is often based on neural networks). This not only provides us with the flexibility of designing and experimenting with different network structures but also prevents the so called ‘curse of dimensionality’, enabling large-scale portfolio management.


Deep Deterministic Policy Gradient (DDPG) is a com- bination of Q-learning and policy gradient and succeeds in using a neural network as its function Wapproximator based on Deterministic Policy Gradient Algorithms (Liang, 2014). Proximal Policy Optimization (PPO) is a policy gra- dient method based on Trust Region Policy Optimization (TRPO) (Silver, 2014).


## 3. Approach: a virtual stock market with multiple agents
We developed a framework that accommodates the com- plexity of stock market work flows. The framework con- sists of the following three components:
- **Agents:** These are abstraction of the portfolio man- agers in the real world. An RL agent takes an input S = (Pt,wt) and returns an output A = (wt+1), where Pt is a vector of asset prices pt, relative to pt�1. Assets consist of cash and N stocks, mak- ing it a total of N + 1 instruments. wt is a vector of asset weights at time step t and wt+1 is a vec- tor of asset weights at time step t + 1. A constraint is imposed on the weight vectors such that its ele- ments sum up to one. The RL agent is based on the (Adversarial) Policy Gradient (PG) (Liang, 2018) al- gorithm. For a purpose of comparison, two other RL algorithms were implemented on which an RL agent is based: Deterministic Policy Gradient algorithms (Sil- ver, 2014) and Proximal Policy Optimization(PPO) (Schulman, 2017a). In addition, there exist other non- RL agents. Further details on the kinds of agents is described in the following subsections.
- **Environment:** This component manages the general RL algorithm flow. It comprises of the step function which (1) allows agents to take an action, i.e. place an order at a given time step, (2) calculates rewards, and (3) updates (P,wt) and passes it onto the next step, starting from t = 0 until the end of an epoch. The environment facilitates information flow between the components by providing and API for queries such as stock prices between the Agents and the Exchange.
- **Exchange:** This represents a virtual stock market comprising of the Order Book. Specifically, it im- plements order-driven markets (Ananth Madhavan & Wagner, 2016), wherein trades are executed directly with the virtual exchange without intermediate deal- ers. The agents specify a tuple consisting of the bid price and order size when they want to buy stocks and analogously, a tuple consisting of the ask price and or- der size when they wish to sell stocks. This implies that a bid-ask spread is naturally introduced by the agents’ own actions, i.e., while deciding their target portfolio weights.
Our framework, comprising of the three components de- scribed above is distinguished from the ones in literature, for the latter typically rely on historical prices at which trades are matched without introducing the bid-ask spread feature, which is in reality an important factor to to be con- sidered in equity trading.


## Experimental Results

### Policy Gradient(PG) agents with transaction cost of _0bp_
Everyone looks happy.

![tc_0](./PG_value_0bp.png)



### PG agents with transaction cost of _5bp_
Two RL agents found out how to make handsome profits in the end.

![tc_5](./PG_value_5bp.png)



### PG agents with transaction cost of _100bp_
Two RL agents have managed to reserve their money in this extreme trading environment.

![tc_100](./PG_value_100bp.png)
