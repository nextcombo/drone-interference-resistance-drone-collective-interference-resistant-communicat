# [Drone Interference Resistance] Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python Code]

I. Abstract: This invention discloses a drone cluster anti-interference communication method based on meta reinforcement learning, which is to construct an anti-interference communication environment for the drone cluster, initialize network parameters, and generate multiple meta reinforcement learning tasks; sample tasks from the meta training set, initialize the environment, experience pool, and context sampling pool; each drone inputs observation data interacting with the environment into the encoder and long short-term memory network module for context task inference, obtaining context task information and historical memory, and concatenates it with the current observation of the agent as input to the dual deep recurrent Q network (DDRQN), predicts actions, and interacts with the environment; finally, determine whether the environment has reached the maximum training step length, if so, end the current round, enter the meta testing phase.Otherwise, repeat the next round of training. The invention has the advantages of strong scalability, strong generalization performance, high learning efficiency, strong stability, low transmission energy loss, less hopping overhead, and long network life.
II. Summary with Figures (flowchart) 1
Start by initializing network parameters and generating multiple meta-reinforcement learning tasks.
2  
From the task of sampling from the training set, initialize the environment, experience pool, and context sampling pool.
3  
Randomly generated small amounts of data facilitate sampling, and empty hidden states are used as input for the Long Short-Term Memory network.
4  
Obtain observations from the environment, input a context-aware module based on meta-learning.
5  
Concatenate context vectors, make decisions by the agent, save trajectory and train the model.
6  
Determine whether the task has been completed or reached the maximum step length: if not, return to continue; if so, determine whether it has converged.
7  
If not converged, return to the sampling task step from the meta training set; if converged, enter the meta testing phase and finalize.
III. Claims 1. UAV Collective Interference-Resistant Communication Method Based on Meta-Reinforcement Learning (Core Steps) This method includes the following steps:
•  
Step 1: Constructing an anti-interference communication environment for a drone fleet.
•  
Step 2: Initialize network parameters and generate multiple meta-reinforcement learning tasks.
•  
Step 3: Sample tasks from the meta-training set, initialize the environment, experience pool, and context sampling pool.
•  
Step 4: Each drone inputs observation data interacting with the environment into the encoder and long short-term memory network module for contextual task inference. The distribution outputted by the encoder is sampled to obtain contextual task information, and the hidden state of the long short-term memory network serves as a historical memory for the current task.
•  
Step 5: Concatenate the contextual task information and historical memory with the current observations of the agent, serving as input for DDRQN to predict actions and interact with the environment.
•  
Step 6: Determine whether the environment has reached the maximum training step length. If it has, end this round and reset the multi-agent unmanned aerial vehicle anti-interference environment, initialize the experience pool and sampling pool, and enter the meta-testing phase; otherwise, repeat steps 3 to start the next training round.
2. Step 1: The construction of the anti-interference communication environment for a drone cluster includes the network model, interference model, and communication model, specifically as follows:
•  
(1) Network Model: It consists of N interference machines and M drone clusters, each drone cluster including one cluster head and I cluster members. The interference machines move according to the Markov Random Walk model, while the cluster members move according to the Reference Point Group movement model (where reference points move within a circular area centered at the cluster head with a radius equal to R1, and cluster members move within a circular area centered at their respective reference points with a radius equal to R2), and satisfy the condition R1 + R2 â¤Dmax (where Dmax is the maximum communication distance).
•  
(2) Disruption Model: The disruptor employs a Markovian disruption model, with the channel transition probability (for the total number of channels) initialized to a uniform distribution from [0,1]. The diverse reinforcement learning tasks are generated through the combination of operations A (randomly selecting a channel and adjusting the transition probabilities), operation B (performing operation A based on random numbers and thresholds), and operation C (normalization).
•  
(3) Communication Model: Time is divided into T time slots (each slot duration), and the power gain consists of slow fading and fast fading.
•  
Fast fading gain (short-term fading, influenced by multipath effects):
Among them, for the standard normal distribution (mean 1, variance 0), for the imaginary unit, for the modulus of a complex number, the results are in dB.
•  
Slow fading gain (long-term fading, influenced by distance and environmental obstruction):
Among them, the path loss constant at 1m distance (unit: dB), the real-time distance between cluster members and cluster head (unit: m), and the attenuation index (2~3 in open environment, 3~5 in dense obstruction environment).
•  
Total channel power gain: (unit: dB).
•  
Interference calculation: (including inter-cluster, interference machine, intra-cluster interference);
•  
Data transmission rate: (Channel bandwidth, Gaussian Additive White Noise Power).
3. Step 2: Network Parameter Initialization and Meta-Reinforcement Learning Task Generation
Model the optimization problem as a partially observable Markov decision process:
•  
Cluster head m's observation value in slot t: (The power gain of the cluster head and its members across all channels, for interference machine channel selection).
•  
Cluster head m's action in slot t: (for transmission channel selection, for transmit power);
•  
Strategy: (To observe the probability of selecting an action);
•  
Transmission energy loss: (for actual transmission time);
•  
Frequency hopping overhead: (unit frequency hopping cost);
•  
Local Rewards: (As a auxiliary variable, as a success reward, as the proportion of energy consumption);
•  
Utilize the interference model to generate diverse reinforcement learning tasks through combinations of A, B, and C.
4. Step 3: Task Sampling and Pool Initialization Stores the transitions in the experience pool and context sampling pool, with each cluster head m's pool being independent.
5. Step 4: Observation Data Processing (Encoder and LSTM Module)
Step 4.1: Develop a contextual meta-learning framework based on DDRQN, sampling data inputs from the context sampler and encoding them into the encoder, feeding hidden states and current observations to an LSTM. The input for DDRQN is the concatenation of the encoder vector z and the output of the LSTM.
•  
Step 4.2: Evaluate the posterior distribution using a reasoning network, modeled as (normal distribution, estimated by neural networks for mean and variance); -
•  
Step 4.3: LSTM forget gate calculation (parameters, bias).
6. Step 5: DDRQN Decision and Interaction •
•  
Action Value Function: Evaluate the network (Evaluated value), target network (Evaluated value);
•  
Loss function: The Adam algorithm optimizes the evaluation network parameters, and the target network parameters are periodically copied from the evaluation network.
7. Step 6: During the meta-testing phase, pre-trained model parameters are used on the meta-test set to test. During this process, parameters are fine-tuned to verify the capability of quickly adapting to new environments and the final convergence effect.
8. Enhanced drone communication device based on meta-reinforcement learning for anti-interference in aerial vehicles, which is used to implement the above method, including:
•  
Environment Constructing Module: Build the anti-interference communication environment for drone cluster.
•  
Initialization Module: Initialize network parameters and generate multiple meta-reinforcement learning tasks.
•  
Task Sampling Module: Sample tasks from the meta-training set, initialize environment, experience pool, and context sampling pool.
•  
Observation Data Processing Module: Process observation data to obtain contextual task information and historical memory.
•  
Joint module: Inputs the joint information and DDRQN, predicts actions and interacts;
•  
Judgement module: Judge the training step length, decide whether to enter the meta-testing phase.
9. Mobile terminals include memory, processor, and computer programs, wherein the processor implements the aforementioned communication method when executing the program.
10. Computer-readable storage medium having stored thereon a computer program which, when executed by the processor, implements the steps of the above communication method.
IV. 1. Technical Field The invention belongs to the field of intelligent communication technology for unmanned aerial vehicles. Involvement of a method for anti-interference communication in drone cluster based on meta-reinforcement learning.
2. Background Technology•
Drone swarm communication faces challenges such as malicious interference, scarce spectrum resources, and limited battery capacity. Traditional anti-interference solutions cannot adapt to dynamic interference patterns.
•  
Existing technologies (such as Q-learning, multi-agent reinforcement learning, dual deep Q-learning) require retraining for new tasks, which cannot adapt to changes in the environment and task.
•  
Meta-learning (such as MAML, 2RL, PEARL algorithms) can quickly adapt to new tasks and provide solutions for drone communication interference resistance.
3. Invention Content •
Objective: To provide a solution for new tasks from limited experience. Highly transferable. High efficiency of sample. Strong anti-interference performance. Low transmission energy consumption. A communication method with small frequency hopping overhead;
•  
Core Method: Consistent with the steps described in claim 1 (building environment, initializing parameters and tasks, sampling training, data processing, decision interaction, phase judgment);
•  
Device and storage medium: consistent with the claims 8-10. -
4. Invention Advantages (1) Highly modular Extendable. Easy to integrate into existing algorithms; (2) The meta-learning mechanism enhances the generalization performance and learning efficiency. Effective convergence; (3) Lower the transmission energy consumption and frequency hopping overhead. Prolong the lifespan of the network.
5. Specific implementation examples
Experimental setup:
•  
Comparative Algorithms: Context-aware DDRQN meta-learning anti-interference algorithm, DDRQN anti-interference algorithm, and random action meta-learning anti-interference algorithm.
•  
Task generation: 300 tasks (combinations of a-d), 200 training, 100 testing;
•  
Parameters: Maximum step size in the meta training phase is 200 (update parameters every 10 steps), maximum step size in the meta testing phase is 100 (update parameters every 40 steps), other parameters are shown in Table 1; -
•  
Experimental results:
•  
During the training phase, the DDRQN meta-learning algorithm based on contextual awareness converges more robustly (encoder and LSTM enhance task inference and memory capabilities).
•  
During the initial testing phase, this algorithm exhibits faster convergence speed, higher communication success rewards, reduced transmission power, and lower frequency hopping overhead, thereby extending network lifespan.
V. Instructions and Illustrations (illustrative results)
Figure 1: Method flowchart;
•  
Figure 2: Accumulated round rewards converge during the training phase;
•  
Figure 3: Accumulated round rewards converged during the meta-testing phase;
•  
Figure 4: Accumulated energy loss convergence effect during the initial test phase;
•  
Figure 5: Accumulated frequency hopping overhead convergence effect during the test phase.
•  
Figure 6: Accumulated communication success reward convergence effect during the test phase.
End

## Images

![(Drone Interference Resistance)Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python code]](01.jpg)
![(Drone Interference Resistance)Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python code]](02.jpg)
![(Drone Interference Resistance)Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python code]](03.jpg)
![(Drone Interference Resistance)Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python code]](04.jpg)
![(Drone Interference Resistance)Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python code]](05.jpg)
![(Drone Interference Resistance)Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python code]](06.jpg)
![(Drone Interference Resistance)Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python code]](07.jpg)
![(Drone Interference Resistance)Drone Collective Interference Resistant Communication Based on Meta-Reinforcement Learning [with Python code]](08.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
