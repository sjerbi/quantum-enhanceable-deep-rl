# Quantum enhancements for deep reinforcement learning in large spaces

The git repository quantum-enhanceable-deep-rl accompanies the paper "Quantum enhancements for deep reinforcement learning in large spaces" [PRXQuantum.2.010328](https://journals.aps.org/prxquantum/abstract/10.1103/PRXQuantum.2.010328). The main class that is introduced in this repository is called `class DEBNAgent`. This is an instance of an energy-based model used as a function approximator to learn a merit function. Updates to the merit function are, for example, performed by using standard value-based methods, such as SARSA, or by using projective simulation. The DEBN can provide learning advantages over standard DQNs in complex RL environments. These advantages, demonstrated in Figures 7-9 of the paper, come at the cost of inefficient sampling from energy-based policies, needed to act on the environment and train the RL agent. However, sampling, as well as the evaluation of the approximate merit function and its gradient, are amenable to quantum speed-ups. In the following section, we describe the functions and classes that need to be adapted to integrate quantum subroutines.  

# Pseudo-code of the hybrid algorithm

The algorithm implemented in this repository does not yet leverage any quantum speed-ups. Algorithm 1, as shown below, is a hybrid quantum-classical algorithm, where Theorems 1, 2, and 3 highlight the quantum subroutines that can be implemented to gain speed-ups.

![Pseudo-code-highlighted](https://user-images.githubusercontent.com/33830063/103460125-1d3aa780-4d14-11eb-900b-2e87a4fe88e3.jpg)

### Theorem 1: 
The sampling process can be sped up by replacing the `_choose_action` function of the DEBNAgent class with a quantum sampling algorithm. The type of sampling algorithm can be chosen in dependence on the used energy-based model, as discussed in detail in the paper.

### Theorem 2 and 3: 
DEBNs do not benefit from quantum speed-ups for the evaluation of the merit function they approximate and its gradient (since these two subroutines can be efficiently implemented using feed-forward evaluation and backpropapagation of deep neural networks). When replacing the `class DEBNAgent` by a new class, for example, based on Boltzmann machines (DBM) or quantum Boltzman machines (QBM), speeding-up the evaluation of the merit function and its gradient is possible. An implementation of a new DBMAgent class or a new QBMAgent class would entail the implementation of their associated functions (currently called `forward` and `backward` functions for DEBNs). 

# Runfiles

``` run_debn_gridworld.py ```

To reproduce the results of Figure 6a: run command lines of the form <br/> `python run_debn_gridworld.py --agent_number (int)`<br/>
e.g. `python run_debn_gridworld.py --agent_number 0` <br/>
Run code with --agent_number n for n in {0,..,50} to gather data for 50 different agents. 

``` run_tabular_gridworld.py ```

To reproduce the results of Figure 6a: run command lines of the form <br/> `python run_tabular_gridworld.py --agent_number (int)` <br/>
e.g. `python run_tabular_gridworld.py --agent_number 0` <br/>
Run code with --agent_number n for n in {0,..,50} to gather the statistics for 50 different agents. 

``` run_debn_cartpole.py ```

To reproduce the results of Figure 6b: run command lines of the form <br/>
`python run_debn_cartpole.py --hidden_layers (int) --hidden_units (int) --learning_rate (float) --target_update (int) --batch_size (int) --agent_number (int)` <br/>

e.g. `python run_debn_cartpole.py --hidden_layers 1 --hidden_units 73 --learning_rate 0.01 --target_update 5000 --batch_size 200 --agent_number 0` <br/>
     `python run_debn_cartpole.py --hidden_layers 2 --hidden_units 19 --learning_rate 0.01 --target_update 5000 --batch_size 200 --agent_number 0` <br/>
     `python run_debn_cartpole.py --hidden_layers 5 --hidden_units 10 --learning_rate 0.001 --target_update 10000 --batch_size 100 --agent_number 0`<br/> 
Run code with --agent_number n for n in {0,..,50} to gather the statistics for 50 different agents. 

``` run_debn-dqn_compare.py ```

To reproduce the results of Figure 7 and Figure 8a: run command lines of the form <br/> `run_debn-dqn_compare.py {4-10} {False-True} 20/$i {1->$i}` <br/>
e.g. `python run_debn-dqn_compare.py 4 True 20 0`

``` run_circular_gridworld_reward-discounting.py ```

To reproduce the results of Figure 8b: run command lines of the form <br/> `run_circular_gridworld_reward-discounting.py {sarsa-ps} {5-9-13-17-21-25} {0-0.9} True 20/$i {1->$i}` <br/>
e.g. `python run_circular_gridworld_reward-discounting.py sarsa 5 0.9 True 20 1`

``` run_circular_gridworld_full-RL.py ```

To reproduce the results of Figure 9: run command lines of the form <br/> `python run_circular_gridworld_full-RL.py {sarsa-ps} {debn-dqn} {17-21-25-29} 0.9 True {0->19}` <br/>
e.g. `python run_circular_gridworld_full-RL.py sarsa dqn 17 0.9 True 0`


# Directories

``` results ```
The results produced by any of the runfiles are saved in this directory. 

``` saved_models ```
The state dictionaries of the agents are stored in this folder if the Boolean value SAVE_MODEL is set to True.  
