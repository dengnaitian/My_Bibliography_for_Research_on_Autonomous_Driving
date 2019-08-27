# My_Bibliography_for_Research_on_Autonomous_Driving

! WORK IN PROGRESS !

I reference additional publications in my other works:

- [Hierarchical Decision-Making for Autonomous Driving](https://github.com/chauvinSimon/Hierarchical-Decision-Making-for-Autonomous-Driving)
- [Educational application of Hidden Markov Model to Autonomous Driving](https://github.com/chauvinSimon/hmm_for_autonomous_driving)
- [My 10 takeaways from the 2019 Intelligent Vehicle Symposium](https://github.com/chauvinSimon/IV19)

## Behavior Cloning, End-To-End (E2E), Imitation Learning

Codevilla, F., Santana, E., Antonio, M. L., & Gaidon, A. [2019].
**"Exploring the Limitations of Behavior Cloning for Autonomous Driving"**
[[pdf](https://arxiv.org/abs/1904.08980)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![Conditional Imitation Learning is extended with a ResNet architecture and Speed prediction (`CILRS`). [Source](https://arxiv.org/abs/1904.08980).](media/2019_codevilla.PNG "Conditional Imitation Learning is extended with a ResNet architecture and Speed prediction (`CILRS`). [Source](https://arxiv.org/abs/1904.08980).")  |
|:--:|
| *Conditional Imitation Learning is extended with a ResNet architecture and Speed prediction (`CILRS`). [Source](https://arxiv.org/abs/1904.08980).* |

- Some related concepts:
  - `distributional shift problem`, `off-policy data collection`, [`CARLA`](http://carla.org/), `conditional imitation learning`, `residual architecture`, `reproducibility issue`, `variance caused by initialization and sampling`
- One term: **“CILRS”** = **Conditional Imitation Learning** extended with a **ResNet** architecture and **Speed prediction**.
- One Q&A: _How to include in E2E learning information about the destination, i.e. to disambiguate imitation around multiple types of intersections?_
  - Add a high-level `navigational command` (e.g. _take the next right_, _left_, or _stay in lane_) to the tuple <`observation`, `expert action`> when building the dataset.
- One idea: learn to predict the ego speed ([`mediated perception`](http://deepdriving.cs.princeton.edu/paper.pdf)) to address the _inertia problem_ stemming from [**causal confusion**](https://arxiv.org/pdf/1905.11979.pdf) (**biased correlation** between _low speed_ and _no acceleration_ - when the ego vehicle is stopped, e.g. at a red traffic light, the probability it stays static is indeed overwhelming in the training data).
- Another idea: The off-policy (expert) driving demonstration is not produced by a human, but rather generated from an **omniscient "AI" agent**.
- One quote:

> "The more common the vehicle model and color, the better the trained agent reacts to it. This raises ethical challenges in automated driving".
</details>

## Inverse Reinforcement Learning, Inverse Optimal Control

Sierra Gonzalez, D. [2019].
**"Towards Human-Like Prediction and Decision-Making for Automated Vehicles in Highway Scenarios"**
[[pdf](https://tel.archives-ouvertes.fr/tel-02184362/document)]

<details>
  <summary>Click to expand</summary>

- Note:
  - this **`190`-page thesis** is also referenced in the sections for **prediction** and **planning**.
  - I really like how the author organizes synergies between three modules that are split and made indepedent in most modular architectures:
    - **`(1)` driver model**
    - **`(2)` behavior prediction**
    - **`(3)` decision-making**

- Some related concepts:
  - `Maximum Entropy IRL`

- Related work: there are close concepts to the approach of `(Kuderer et al., 2015)` referenced below.
- One idea: **encode the driving preferences** of a human driver with a **reward function** (or **cost function**), mentioning a quote from Abbeel, Ng and Russell:

> “The reward function, rather than the policy or the value function, is the most succinct, robust, and transferable definition of a task”.

- Other ideas:
  - Use IRL to **avoid the manual tuning** of the parameters of the reward model. Hence learn a cost/reward function from demonstrations.
  - Include **dynamic features**, such as the `time-headway`, in the linear combination of the cost function, to take the **interactions** between traffic participants into account.
  - Combine IRL with a trajectory planner based on **_"conformal spatiotemporal state lattices"_**.
    - The motivation is to deal with _continuous_ state and action spaces and handle the presence of _dynamic obstacles_.
    - Several advantages (_I honnestly did not understand that point_): the ability to exploit the structure of the environment, to **consider time as part of the state-space** and respect the non-holonomic motion constraints of the vehicle.

- One term: **"_planning-based motion prediction_"**.
  - The resulting reward function can be used to **generate trajectory** (for prediction), using optimal control.
  - Simply put, it can be assumed that each vehicle in the scene behaves in the **"risk-averse"** manner **encoded by the model**, i.e. choosing actions leading to the lowest cost / highest reward.
  - This method is also called "**model**-based prediction" since it relies on a reward function or on the models of a MDP.
  - This prediction tool is not used alone but rather coupled with some **DBN-based maneuver estimation** (detailed in the [section on prediction](#Prediction_and_Manoeuvre_Recognition)).

</details>

Kuderer, M., Gulati, S., & Burgard, W. [2015].
**"Learning driving styles for autonomous vehicles from demonstration"**
[[pdf](http://ais.informatik.uni-freiburg.de/publications/papers/kuderer15icra.pdf)]

<details>
  <summary>Click to expand</summary>

- Some related concepts:
  - `inverse optimal control`, `IRL`
- One term: **"ME-IRL"** = **Maximum Entropy** IRL. The probability distribution over trajectories is in the form `exp`(`-cost[f(traj), θ]`), to model that **agents are exponentially more likely to select trajectories with lower cost**.
- One Q&A: The trajectory object is first mapped to some feature vector (`speed`, `acceleration` ...). How to then derive a cost (or reward) from these features?
  - The authors assume the cost function to be a **linear combination of the features**. The goal is then to **learn the weights**. But they acknowledge in the conclusion that it may be a too simple model. Maybe **neural nets** could help to capture some more complex relations.
- One quote about the _maximum likelihood approximation_ in ME-IRL:

> "We assume that the demonstrations are in fact generated by minimizing a cost function (IOC), in contrast to the assumption that demonstrations are samples from a probability distribution (IRL)".
</details>

He, X., Xu, D., Zhao, H., Moze, M., Aioun, F., & Franck, G. [2018].
**"A Human-like Trajectory Planning Method by Learning from Naturalistic Driving Data"**
[[html](https://ieeexplore.ieee.org/document/8500448)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![[Source](https://ieeexplore.ieee.org/document/8500448).](media/2018_he.PNG "[Source](https://ieeexplore.ieee.org/document/8500448).")  |
|:--:|
| *[Source](https://ieeexplore.ieee.org/document/8500448).* |

- Some related concepts:
  - `sampling-based trajectory planning`
- One idea: couple **_learning_** and **_sampling_** for **motion planning**.
  - More precisely, learn from human demonstrations (offline) how to **weight** different contributions in a **cost function** (as opposed to _hand-crafted_ approaches).
  - This cost function is then used for **trajectory planning** (online) to evaluate and select one trajectory to follow, among a set of candidates generated by **sampling methods**.
- One detail: the weights of the optimal cost function minimise the sum of [`prob`(`candidate`) * `similarities`(`demonstration`, `candidate`)].
  - It is clear to me how a _cost_ can be converted to _some probability_, using `softmax()`.
  - But for the **similarity measure** of a trajectory candidate, how to compute "its distance to the human driven one at the _same driving situation_"?
  - Should the expert car have driven _exactly on the same track_ before or is there any abstraction in the representation of the _situation_?
  - How can it **generalize at all** if the similarity is estimated only on the **location** and **velocity**? The traffic situation will be different between two drives.
- One quote:

> "The more similarity (i.e. less distance) the trajectory has with the human driven one, the higher probability it has to be selected."

</details>

## Prediction and Manoeuvre Recognition

Liu, S., Zheng, K., Member, S., Zhao, L., & Fan, P. [2019].
**"A Driving Intention Prediction Method Based on Hidden Markov Model for Autonomous Driving"**
[[pdf](https://arxiv.org/abs/1902.09068)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![[Source](https://arxiv.org/abs/1902.09068).](media/2019_lui.PNG "[Source](https://arxiv.org/abs/1902.09068).")  |
|:--:|
| *[Source](https://arxiv.org/abs/1902.09068).* |

- Some related concepts:
  - `HMM`, `Baum-Welch algorithm`, `forward algorithm`
- One term: **"mobility feature matrix"**
  - The recorded data (e.g. absolute positions, timestamps ...) are processed to form the _mobility feature matrix_ (e.g. speed, relative position, lateral gap in lane ...).
  - Its size is `T × L × N`: `T` time steps, `L` vehicles, `N` types of mobility features.
  - In the _discrete characterization_, this matrix is then turned into a **set of observations** using K-means clustering.
  - In the _continuous case_, mobility features are modelled as Gaussian mixture models (GMMs).
- This work implements HMM concepts presented in my project [Educational application of Hidden Markov Model to Autonomous Driving](https://github.com/chauvinSimon/hmm_for_autonomous_driving).

</details>

Huang, X., Hong, S., Hofmann, A., & Williams, B. [2019].
**"Online Risk-Bounded Motion Planning for Autonomous Vehicles in Dynamic Environments"**
[[pdf](https://arxiv.org/abs/1904.02341)]

<details>
  <summary>Click to expand</summary>

- Some related concepts:
  - `intention-aware planning`, `manoeuvre-based motion prediction`, `POMDP`, `probabilistic risk assessment`, `CARLA`

One figure:

| ![[Source](https://arxiv.org/abs/1904.02341).](media/2019_huang.PNG "[Source](https://arxiv.org/abs/1904.02341).")  |
|:--:|
| *[Source](https://arxiv.org/abs/1904.02341).* |

- One term: [**"Probabilistic Flow Tubes"**](https://dspace.mit.edu/handle/1721.1/76824) (PFT)
  - A *motion representation* used in the **"Motion Model Generator"**.
  - Instead of using **hand-crafted** rules for the transition model, the idea is to **learns human behaviours** from demonstration.
  - The inferred models are encoded with **PFTs** and are used to generate **probabilistic predictions** for both _manoeuvre_ (long-term reasoning) and _motion_ of the other vehicles.
  - The advantage of **belief-based probabilistic planning** is that it can avoid **over-conservative** behaviours while offering **probabilistic safety guarantees**.
- Another term: **"Risk-bounded POMDP Planner"**
  - The **uncertainty** in the intention estimation is then propagated to the decision module.
  - Some notion of **risk**, defined as the _probability of collision_, is evaluated and considered when taking actions, leading to the introduction of a **"chance-constrained POMDP"** (`CC-POMDP`).
  - The **online solver** uses a heuristic-search algorithm, [**Risk-Bounded AO\***](https://www.aaai.org/ocs/index.php/AAAI/AAAI16/paper/download/12321/12095) (**RAO\***), takes advantage of the **risk estimation** to prune the over-risky branches that violate the **risk constraints** and eventually outputs a plan with a **guarantee over the probability of success**.
- One quote (_this could apply to many other works_):

> "One possible future work is to test our work in real systems".

</details>

Sierra Gonzalez, D. [2019].
**"Towards Human-Like Prediction and Decision-Making for Automated Vehicles in Highway Scenarios"**
[[pdf](https://tel.archives-ouvertes.fr/tel-02184362/document)]

<details>
  <summary>Click to expand</summary>

- Some related concepts:
  - `planning-based motion prediction`, `manoeuvre-based motion prediction`

- Prediction techniques are often classified into three types:
  - `physics-based`
  - `manoeuvre-based` (and `goal-based`).
  - `interaction-aware`

- As I understood, the main idea here is to **combine prediction techniques** (and their advantages).
  - The **driver-models** (i.e. the reward functions previously learnt with IRL) can be used to identify the most likely, risk-aversive, anticipatory maneuvers. This is called the `model-based` prediction by the author since it relies on one _model_.
    - But relying only on **driver models** to predict the behavior of surrounding traffic might fail to predict dangerous maneuvers.
    - As stated, _"the model-based method is not a reliable alternative for the_ **_short-term_** estimation of behavior, since it cannot predict_ **_dangerous actions that deviate_** _from_ **_what is encoded in the model_**_"_.
    - One solution is to add a term that represents **how the observed movement of the target _matches_ a given maneuver**.
    - In other words, to **consider the noisy observation of the dynamics of the targets** and include theses so-called `dynamic evidence` into the prediction.

- Usage:
  - The resulting approach is used in the _probabilistic filtering framework_ to **update the belief** in the POMDP and in its **rollout** (to bias the **construction of the history tree** towards likely situations given the state and intention estimations of the surrounding vehicles).
  - It improves the inference of manoeuvers, **reducing rate of false positives** in the detection of lane change maneuvers and enables the exploration of situations in which the surrounding vehicles behave dangerously (not possible if relying on **safe generative models** such as `IDM`).

- One quote about this combination:

> "This model mimics the reasoning process of human drivers: they can guess what a given vehicle is likely to do given the situation (the **model-based prediction**), but they closely **monitor its dynamics** to detect deviations from the expected behavior".

- One idea: use this combination for **risk assessment**.
  - As stated, _"if the_ **_intended_** _and_ **_expected_** _maneuver of a vehicle_ **_do not match_**_, the situation is classified as dangerous and an alert is triggered"_.
  - This is a important concept of **risk assessement** I could [identify at IV19](https://github.com/chauvinSimon/IV19#risk-assessment-and-safety-checkers): a situation is dangerous if there is a discrepency between _what is expected_ (given the context) and _what is observed_.

- One term: **"_Interacting Multiple Model_"** (`IMM`), used as baseline in the comparison.
  - The idea is to consider a **group of motion models** (e.g. `lane keeping with CV`, `lane change with CV`) and continuously estimate which of them captures more accurately the dynamics exhibited by the target.
  - The final predictions are produced as a **weighted combination** of the **individual predictions of each filter**.
  - `IMM` belongs to the **_physics-based_** predictions approaches and could be extended for `maneuver inference` (called _dynamics matching_). It is often used to **maintain the beliefs** and **guide the observation sampling** in POMDP.
  - But the issue is that IMM completely **disregards the interactions between vehicles**.

</details>

Li, S., Li, N., Girard, A., & Kolmanovsky, I. [2019].
**"Decision making in dynamic and interactive environments based on cognitive hierarchy theory: Formulation, solution, and application to autonomous driving"**
[[pdf](https://arxiv.org/abs/1908.04005)]

<details>
  <summary>Click to expand</summary>

- Some related concepts:
  - `level-k game theory`, `cognitive hierarchy theory`, `interaction modelling`, `interaction-aware decision making`
- One concept: **`cognitive hierarchy`**.
  - Other drivers are assumed to follow some **"cognitive behavioral models"**, parametrized with a so called **"_cognitive level_"** `σ`.
  - The goal is to **obtain and maintain belief about `σ`** based on observation in order to **optimally respond** (using a `MPC`).
  - Three levels are considered:
    - level-`0`: driver that treats other vehicles on road as **stationary obstacles**.
    - level-`1`: **cautious/conservative** driver.
    - level-`2`: **aggressive** driver.

- One quote about the **"_cognitive level_"** of human drivers:

> "Humans are most commonly level-1 and level-2 reasoners".

Related works:

- Li, N., Oyler, D., Zhang, M., Yildiz, Y., Kolmanovsky, I., & Girard, A. [2016]. **"Game-theoretic modeling of driver and vehicle interactions for verification and validation of autonomous vehicle control systems"** [[pdf](https://arxiv.org/abs/1608.08589)]
  - > "If a driver assumes that the other drivers are level-`1` and **takes an action accordingly**, this driver is a level-`2` driver".
  - Use RL with **hierarchical assignment** to learn the policy:
    - First, the `π-0` (for level-`0`) is learnt for the ego-agent.
    - Then `π-1` with all the other participants following `π-0`.
    - Then `π-2` ...
  - **Action masking**: "If a car in the _left lane_ is in a parallel position, the controlled car _cannot change lane to the left_".
    - _"The use of these_ _**hard constrains**_ _eliminates the clearly undesirable behaviors better than through penalizing them in the reward function, and also_ **_increases the learning speed during training_**_"_

- Ren, Y., Elliott, S., Wang, Y., Yang, Y., & Zhang, W. [2019]. **"How Shall I Drive ? Interaction Modeling and Motion Planning towards Empathetic and Socially-Graceful Driving"** [[pdf](https://arxiv.org/abs/1901.10013)] [[code](https://github.com/scaperoth/carmachinelearning)]

| ![[Source](https://arxiv.org/abs/1901.10013).](media/2019_ren_1.PNG "[Source](https://arxiv.org/abs/1901.10013).")  |
|:--:|
| *[Source](https://arxiv.org/abs/1901.10013).* |

| ![[Source](https://arxiv.org/abs/1901.10013).](media/2019_ren_2.PNG "[Source](https://arxiv.org/abs/1901.10013).")  |
|:--:|
| *[Source](https://arxiv.org/abs/1901.10013).* |

</details>

## Rule-based Decision Making

Naumann, M., Königshof, H., & Stiller, C. [2019].
**"Provably Safe and Smooth Lane Changes in Mixed Traffic"**
[[pdf](https://www.mrt.kit.edu/z/publ/download/2019/Naumann2019LaneChange.pdf)]
[[video](https://www.mrt.kit.edu/z/publ/download/2019_LaneChange_Naumann.mp4)]
[[Code for Simulator](https://github.com/coincar-sim)]

<details>
  <summary>Click to expand</summary>

Some figures:

| ![The first `safe?` check might lead to conservative behaviours (huge gaps would be needed for safe lane changes). Hence it is relaxed with some `Probably Safe?` condition. [Source](https://www.mrt.kit.edu/z/publ/download/2019/Naumann2019LaneChange.pdf).](media/2019_naumann_1.PNG "The first `safe?` check might lead to conservative behaviours (huge gaps would be needed for safe lane changes). Hence it is relaxed with some `Probably Safe?` condition. [Source](https://www.mrt.kit.edu/z/publ/download/2019/Naumann2019LaneChange.pdf).")  |
|:--:|
| *The first `safe?` check might lead to conservative behaviours (huge gaps would be needed for safe lane changes). Hence it is relaxed with some `Probably Safe?` condition. [Source](https://www.mrt.kit.edu/z/publ/download/2019/Naumann2019LaneChange.pdf).* |

| ![[Source](https://www.mrt.kit.edu/z/publ/download/2019/Naumann2019LaneChange.pdf).](media/2019_naumann_2.PNG "[Source](https://www.mrt.kit.edu/z/publ/download/2019/Naumann2019LaneChange.pdf).")  |
|:--:|
| *[Source](https://www.mrt.kit.edu/z/publ/download/2019/Naumann2019LaneChange.pdf).* |

| ![Formulation by Pek, Zahn, & Althoff, 2017. [Source](https://mediatum.ub.tum.de/doc/1379669/794156.pdf).](media/2017_pek.PNG "Formulation by Pek, Zahn, & Althoff, 2017. [Source](https://mediatum.ub.tum.de/doc/1379669/794156.pdf).")  |
|:--:|
| *Formulation by Pek, Zahn, & Althoff, 2017. [Source](https://mediatum.ub.tum.de/doc/1379669/794156.pdf).* |

- Some related concepts:
  - `path-velocity decomposition`, `IDM`, `RSS`

- Main ideas:
  - The notion of **_safety_** is based on the **responsibility sensitive safety** (`RSS`) definition.
    - As stated by the authors, _"A **`safe`** lane change is guaranteed not to **`cause`** a collision according to the previously defined rules, while a single vehicle cannot ensure that it will never be involved in a collision."_
  - Use _set-based reachability analysis_ to prove the "RSS-safety" of lane change maneuvers based on **gap evaluation**.
    - In other words, it is the **responsibility** of the ego vehicle to **maintain safe distances** during the lane change manoeuvre.

- Related works: A couple of safe distances are defined, building on
  - **`RSS`** principles (_after IV19, I tried to summarize some of the RSS concepts [here](https://github.com/chauvinSimon/IV19#rss)_).
  - [_"Verifying the Safety of Lane Change Maneuvers of Self-driving Vehicles Based on Formalized Traffic Rules"_](https://mediatum.ub.tum.de/doc/1379669/794156.pdf), (Pek, Zahn, & Althoff, 2017)

</details>

Noh, S. [2018].
**"Decision-Making Framework for Autonomous Driving at Road Intersections: Safeguarding Against Collision, Overly Conservative Behavior, and Violation Vehicles"**
[[html](https://ieeexplore.ieee.org/document/8370800)]
[[video](https://www.youtube.com/watch?v=h7ExZ040wyk)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![[Source](https://ieeexplore.ieee.org/document/8370800).](media/2018_noh.PNG "[Source](https://ieeexplore.ieee.org/document/8370800).")  |
|:--:|
| *[Source](https://ieeexplore.ieee.org/document/8370800).* |

- Some related concepts:
  - `probabilistic risk assessment`, `rule-based probabilistic decision making`
- Many ML-based works criticize rule-based approaches (_over-conservative_, _no generalization capability_ and _painful parameter tuning_).
  - True, the presented framework contains **many parameters whose tuning may be tedious**.
  - But this approach just works! At least they **go out of the simulator** and show some experiments on a **real car**.
  - I really like [**their video**](https://www.youtube.com/watch?v=h7ExZ040wyk), especially the multiple camera views together with the `RViz` representation.
  - It can be seen that **probabilistic reasoning** and **uncertainty-aware** decision making are essential for **robustness**.
- One term: **"Time-to-Enter"** (tte).
  - It represents the time it takes a _relevant vehicle_ to reach the potential _collision area_ (CA), from its current position at its current speed.
  - To deal with uncertainty in the measurements, a variant of this heuristic is coupled with a Bayesian network for **probabilistic threat-assessment**.
- One Q&A: What is the difference between **_situation awareness_** and **_situation assessment_**?
  - In **_situation awareness_**, all possible routes are considered for the detected vehicles using a map. The vehicles whose potential route **intersect with the ego-path** are classified as _relevant vehicles_.
  - In **_situation assessment_**, a threat level in {`Dangerous`, `Attentive`, `Safe`} is inferred for each _relevant vehicle_.
- One quote:

> "The existing literature mostly focuses on motion prediction, threat assessment, or decision-making problems, but not all three in combination."

</details>

## Model Free Reinforcement Learning

Tram, T., Batkovic, I., Ali, M., & Sjöberg, J. [2019].
**"Learning When to Drive in Intersections by Combining Reinforcement Learning and Model Predictive Control"**
[[pdf](https://arxiv.org/abs/1908.00177)]

<details>
  <summary>Click to expand</summary>

Two figures:

| ![[Source Left](https://arxiv.org/abs/1908.00177) [Source Right](https://pdfs.semanticscholar.org/fba5/6bac9a41b7baa2671355aa113462d2044fb7.pdf).](media/2019_tram_3.PNG "[Source Left](https://arxiv.org/abs/1908.00177) [Source Right](https://pdfs.semanticscholar.org/fba5/6bac9a41b7baa2671355aa113462d2044fb7.pdf).")  |
|:--:|
| *[Source Left](https://arxiv.org/abs/1908.00177) [Source Right](https://pdfs.semanticscholar.org/fba5/6bac9a41b7baa2671355aa113462d2044fb7.pdf).* |

| ![This work applies the **path-velocity decomposition** and focuses on the longitudinal control. **Three intentions** are considered: aggressive `take-way`, `cautious` (slows down without stopping), and passive `give-way`. [Source](https://arxiv.org/abs/1908.00177).](media/2019_tram_2.PNG "This work applies the **path-velocity decomposition** and focuses on the longitudinal control. **Three intentions** are considered: aggressive `take-way`, `cautious` (slows down without stopping), and passive `give-way`. [Source](https://arxiv.org/abs/1908.00177).")  |
|:--:|
| *This work applies the **path-velocity decomposition** and focuses on the longitudinal control. **Three intentions** are considered: aggressive `take-way`, `cautious` (slows down without stopping), and passive `give-way`. [Source](https://arxiv.org/abs/1908.00177).* |

Since the constraints from the surrounding obstacles become non-convex, we rely on the high-level policy maker to decide

- Some related concepts:
  - `model-free RL`, `MPC`, `Q-Masking`, `POMDP`
- Main idea: **hierarchy** in _learnt_/_optimized_ decision-making.
  - A **high-level decision module** based on RL uses the **feedback from the MPC controller** in the reward function.
  - The MPC controller is also responsible for **handling the comfort** of passengers in the car by **generating a smooth acceleration profile**.
- Previous works:
  - [_"Learning Negotiating Behavior Between Cars in Intersections using Deep Q-Learning"_](http://arxiv.org/abs/1810.10469) - (Tram, Batkovic, Ali, & Sjöberg, 2019)
  - [_"Autonomous Driving in Crossings using Reinforcement Learning"_](https://pdfs.semanticscholar.org/fba5/6bac9a41b7baa2671355aa113462d2044fb7.pdf) - (Jansson & Grönberg, 2017)
  - In particular they reused the concept of **_"actions as Short Term Goals (`STG`)"_**. e.g. _keep set speed or yield for crossing car_ instead of some numerical acceleration outputs.
    - This allows for **comfort** on actuation and **safety** to be **tuned separately**, reducing the policy selection to a classification problem.
    - The use of such abstracted / high-level decisions could be a first step toward **hierarchical RL** techniques (`macro-action` and `option` framework).
  - Another contribution consists in remplacing the **Sliding Mode** (**`SM`**) controller used previously by a `MPC`, allegedly to _"achieve safer actuation by using constraints"_.
    - The **intention of all agents** is implemented with a `SM` controller with **various target values**.
- I find it valuable to have details about the **training phase** (no all papers do that). In particular:
  - The **normalization** of the input features in [`-1`, `1`].
  - The **normalization** of the reward term in [`-2`, `1`].
  - The use of **equal weights** for inputs that describe the state of **interchangeable objects**.
  - Use of a **`LSTM`**, as an alternative to a **DQN with stacked observations**. (Findings from _(Jansson & Grönberg, 2017)_).
- Additional notes:
  - The main benefits of the combination seems to be about **avoiding over conservative behaviours** while improving the **"sampling-efficient"** of the model-free RL approach.
    - Such approachs looks to be particularly relevant (in term of _success rate_ and _collision-to-timeout ratio_ [`CTR`]) for complex scenarios, e.g. `2`-crossing scenarios.
    - For simple cases, the performance stays close to the baseline.
  - The reliance (_and the burden_) on an appropriate **parametrisation** inherent to rule-based has not disappeared and the **generalisation seems limited**:
    - _"Since MPC uses_ **_predefined models_** _, e.g. vehicle models and other obstacle prediction models, the_ **_performance relies on their accuracy and assumptions_** _."_
  - The problem is formulated as a `POMDP`.
    - _Honestly, from a first read, I did not find how `belief tracking` is performed. Maybe something related to the internal memory state of the LSTM cells?_
  - Sadly the **simulator** seems to be **home-made**, which makes reproducibility tricky.
- One quote about **`Q-masking`**, i.e. the technique of not exposing to the agent dangerous or non-applicable actions during the action selection.
  - > "**Q-masking helps the learning process** by **reducing the exploration** space by disabling actions the agent does not need to explore."
  - Hence the agent **does not have to explore** these options, while ensuring a certain level of safety (but this requires another **rule-based module** ;) ).

</details>

Bouton, M., Nakhaei, A., Fujimura, K., & Kochenderfer, M. J. [2019].
**"Cooperation-Aware Reinforcement Learning for Merging in Dense Traffic"**
[[pdf](https://arxiv.org/abs/1906.11021)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![[Source](https://arxiv.org/abs/1906.11021).](media/2019_bouton.PNG "[Source](https://arxiv.org/abs/1906.11021).")  |
|:--:|
| *[Source](https://arxiv.org/abs/1906.11021).* |

- Some related concepts:
  - `POMDP`, `offline RL`, `value-based RL`, `interaction-aware decision making`, `belief state planning`
- One idea: offline **belief state RL** to solve dense merging scenarios modelled as a POMDP.
  - A belief updater explicitly maintains a probability distribution over the **driver cooperation levels** of other cars.
- Another idea: projection of the merging vehicle on the main lane. This reduces the problem to **one dimension**, allowing for IDM. Similar to the [abstraction presented by R. Regele](https://ieeexplore.ieee.org/document/4488328/).
- One term: **"C-IDM"**: Cooperative Intelligent Driver Model.
  - It builds on IDM to control the longitudinal acceleration of the other vehicle and adds a **cooperation level** `c` `∈` [`0`, `1`].
  - Non-cooperative vehicles (`c=0`) are blind to the merging ego car and follow the vanilla IDM, while cooperative vehicles will yield (`c=1`).
- Another term: **"Burn-in time"**
  - When creating a new configuration, vehicles and their parameters are drawn from distributions and then move according to **driver models**.
  - The idea is between `10s` and `20s` before spawning the ego car, to allows the initial state to converge to a **more realistic situation**.
  - It should help for **generalization**.
- Another term: **"Curriculum Learning"**: The idea is to train the agent by **gradually increasing the difficulty** of the problem. In this case the traffic density.
- Two take-aways (similar to what [I identified at IV19](https://github.com/chauvinSimon/IV19))

> "Previous works has shown that only relying on deep RL is not sufficient to achieve safety. The deployment of those policies would require the addition of a safety mechanism."

> "Using deep reinforcement learning policies to guide the search of a classical planner (MCTS) may be a promising direction."

</details>

Hu, Y., Nakhaei, A., Tomizuka, M., & Fujimura, K. [2019].
**"Interaction-aware Decision Making with Adaptive Behaviors under Merging Scenarios"**
[[pdf](https://arxiv.org/abs/1904.06025)]
[[video](https://www.youtube.com/watch?v=2CTTFHDW1ec)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![Note: the visibility of each agent is assumed to be `100m` in front and back, with `0.5m`/`cell` resolution, for both its current lane (`obs_cl`) and the other lane (`obs_ol`). [Source](https://arxiv.org/abs/1904.06025).](media/2019_hu.PNG "Note: the visibility of each agent is assumed to be `100m` in front and back, with `0.5m`/`cell` resolution, for both its current lane (`obs_cl`) and the other lane (`obs_ol`). [Source](https://arxiv.org/abs/1904.06025).")  |
|:--:|
| *Note: the visibility of each agent is assumed to be `100m` in front and back, with `0.5m`/`cell` resolution, for both its current lane (`obs_cl`) and the other lane (`obs_ol`). [Source](https://arxiv.org/abs/1904.06025).* |

- Some related concepts:
  - `multi agent RL`, `interaction-aware decision making`, `curriculum learning`, `action masking`

- One term: **"IDAS"**: interaction-aware decision making with adaptive strategies.
  - The main goal is to generate manoeuvres which are **safe** but **less conservative** than rule-based approaches such as **IDM** and/or FSM.
  - The idea is to learn how to **negotiate** with other drivers, or at least consider **interactions** in the decision process.

- One idea: use multi-agent RL (**MARL**) to consider **interactions** between the multiple road entities.
  - In particular, the agent receives rewards for its **personal objective** as well as for **its contribution to the team’s "success"** (_multi-agent credit assignment_).

- One idea: a **masking mechanism** prevents the agent from **exploring states that violate common sense** of human judgment (c.f. [RSS](https://www.mobileye.com/responsibility-sensitive-safety/)) and increase the **learning efficiency**.
  - This idea of adding **rule-based constraints** to a RL policy has been applied in many works. Recently in [Wang, J. et al.](https://arxiv.org/abs/1904.00231) for instance where prediction is also considered.
  - Here, masking is not only based on **safety**, but also on considers **vehicle kinematics** and **traffic rules**.
  - A remaining question is **_where_ to apply the masking**: either _before_ the **action selection** (exposing only a subset of feasible actions), or _after_ (penalizing the agent if it takes a forbidden action).

- One quote (on the way to _transfer_ to the real world):

> "A motion control module will convert the discretized acceleration of the behaviour planner into continuous acceleration by applying algorithms like MPC at a higher frequency (100Hz)".

</details>

Bacchiani, G., Molinari, D., & Patander, M. [2019].
**"Microscopic Traffic Simulation by Cooperative Multi-agent Deep Reinforcement Learning"**
[[pdf](https://arxiv.org/abs/1903.01365)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![[Source](https://arxiv.org/abs/1903.01365).](media/2019_bacchiani.PNG "[Source](https://arxiv.org/abs/1903.01365).")  |
|:--:|
| *[Source](https://arxiv.org/abs/1903.01365).* |

- Some related concepts:
  - `multi-agent A3C`, `off-policy learning`
- One idea: **"parallel actor-learners"**: to **decorrelate** the sequence of experiences used to update the policy network, a common approach is to sample <`s`, `a`, `r`, `s'`> tuples from a memory buffer (**_experience replay_**).
  - Here, a **multiple-agent** setting is used instead: each agent acts in a different **instance of the environment** (hence diverse experiences) and sends its updates asynchronously to a **central network**.
- Another idea: **"hybrid state representation"**: coupling some _grid-like_ representation (`path to follow`, `obstacles`, `navigable space`) with a vector of _explicit features_ (e.g. `target speed`, `distance to goal`, `elapsed time ratio`).
  - This combination offers a **generic scene representation** (i.e. independent of the number of vehicles) while allowing for tuning explicit goals (`target speeds`) in the state.
  - Such _hybrid_ representation seems popular, as [identified at IV19](https://github.com/chauvinSimon/IV19#generic-scene-representation)).
- Other ideas:
  - Stacking the `n=4` most recent views to _capture the evolution_ of the scene (e.g. relative speeds).
  - **_Action repeat_** technique for _temporal abstraction_ to stabilize the learning process (c.f. "_frame skip_").
- One concept: **"Aggressiveness tuning"**. Together with the `target speed`, the `elapsed time ratio` (ETR) feature is used to tune the aggressiveness of the car:

> "ETR Values close to `1` will induce the agent to drive faster, in order to avoid the predicted negative return for running out of time. Values close to `0` will tell the driver that it still has much time, and it is not a problem to yield to other vehicles."

</details>

Huegle, M., Kalweit, G., Mirchevska, B., Werling, M., & Boedecker, J. [2019].
**"Dynamic Input for Deep Reinforcement Learning in Autonomous Driving"**
[[pdf](https://arxiv.org/abs/1907.10994)]
[[video](https://www.youtube.com/watch?v=mRQgHeAGk2g)]

<details>
  <summary>Click to expand</summary>

- One diagram is better than 100 words:

| ![By summing all the dynamic terms (one per surrounding vehicle), the input keeps a constant size. [Source](https://arxiv.org/abs/1907.10994).](media/2019_huegle.PNG "By summing all the dynamic terms (one per surrounding vehicle), the input keeps a constant size. [Source](https://arxiv.org/abs/1907.10994).")  |
|:--:|
| *By summing all the dynamic terms (one per surrounding vehicle), the input keeps a constant size. [Source](https://arxiv.org/abs/1907.10994).* |

- Some related concepts:
  - `feature engineering`, `off-policy learning`, `DQN`, `SUMO`
- One goal: find a **flexible** and **permutation-invariant** representation to **deal with variable sized inputs** (_variable number of surrounding vehicles_) in high-level decision making in autonomous lane changes, using model-free RL.
  - Four representations are considered:
    - Relational Grid (fixed-size vector)
    - Occupancy grid (fixed-size grid processed by some `CNN`)
    - Set2Set-Q (some `RNN` combined with an _attention mechanism_ to create a set representation which is **permutation invariant** w.r.t. the input elements)
    - DeepSet-Q (proposed approach)
  - They are used as inputs to train and evaluate DQN agents.
- One finding:
  - "`Deep Sets` were able to outperform CNN and **recurrent attention approaches** and demonstrated **better generalization** to unseen scenarios".
- One quote: about the use of **policy-based** (as opposed to _value-based_), **on-policy** (as opposed to _off-policy_), model-free RL (here `PPO`).

> "Due to the higher demand for training and the non-trivial application to autonomous driving tasks of `on-policy` algorithms, we switched to `off-policy` Q-learning."

</details>

Jaâfra, Y., Laurent, J.-L., Deruyver, A., & Naceur, M. S. [2019].
**"Seeking for Robustness in Reinforcement Learning : Application on Carla Simulator"**
[[pdf](https://openreview.net/pdf?id=Bke03G85DN)]

<details>
  <summary>Click to expand</summary>

- Some background:

| ![`n`-step TD learning. [Source](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html).](media/2015_silver.PNG "`n`-step TD learning. [Source](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html).")  |
|:--:|
| *`n`-step TD learning. [Source](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html).* |

- Some related concepts:
  - `CARLA`, `A2C`
- One related work: reading this paper reminded me of one conclusion of the 2019 [CARLA AD Challenge](https://carlachallenge.org/):

> "**Robust** open-source AV stacks are not a commodity yet: **No public AV stack has solved the challenge yet**."

- One idea: use an actor-critic architecture with **multi-step returns** (`n`-step `A2C`) to _"achieve a better robustness"_.
  - The introduction of a **critic** aims at **reducing the variance** of the **gradient** of _policy-based_ methods.
  - As illustrated in the above figure, in _value-based_ methods, the **TD-target** of a critic can be computed in several ways:
    - With boostrapped, using the current estimate for the next state `s'`: `1`-step TD - **low variance** but biased estimate ...
    - ... Up to considering all the steps in the trajectory until termination: `Monte Carlo` - high variance but **unbiased estimate**.
    - In between are **multi-step returns critics** (`MSRC`). Obviously a trade-off between **bias**/**variance**.
- Some limitations:
  - The `MDP` is not very detailed, making **reproduction and comparison impossible**.
    - For instance, the `action` space allegedly contains `3` discrete driving instructions [`steering`, `throttle`, and `brake`], but not concrete number is given.
    - The same applies to the `state` and `reward` function: no quantitative description.
    - Based on their text, I can assume the authors were participating to the 2019 CARLA AD challenge. Maybe [track `1`](https://carlachallenge.org/challenge/). But again, information about the _town_/_scenario_ is missing.
  - No real comparison is performed: it should be feasible to use the built-it **rule-based agent** present in CARLA as a **baseline**.
  - _Why not also supplying a video of the resulting agent?_
- One additional work: **_Meta-Reinforcement Learning for Adaptive Autonomous Driving_**, (Jaafra, Luc, Aline, & Mohamed, 2019) [[pdf](https://openreview.net/pdf?id=S1eoN9rsnN)] [[poster](https://www.ds3-datascience-polytechnique.fr/wp-content/uploads/2019/06/DS3-552_2019.pdf)].
  - This idea is to use **multi-task learning** to improve **generalization** capabilities for a AD controller.
  - As detailed, _"Meta-learning refers to **learn-to-learn** approaches that aim at **training** a model on a set of different but linked tasks and subsequently **generalize** to new cases using **few additional examples**"_.
  - In other words, the goal is to find an optimal **initialization of parameters**, to then quickly adapt to a new task through a few standard gradient descents(**few-shot generalization**).
  - A **gradient-based meta-learner** inspired from **_Model-Agnostic Meta-Learning_** (`MAML` - Finn et al., 2017) is used.
  - RL performance in **non-stationary** environments and generalisation in AD are interesting topics. But no clear benefit is demonstrated, and the above limitations apply also here.

</details>

## Planning and Monte Carlo Tree Seach

Lee, J., Balakrishnan, A., Gaurav, A., & Feb, L. G. [2019].
**"WiseMove: A Framework for Safe Deep Reinforcement Learning for Autonomous Driving"**
[[pdf](https://arxiv.org/abs/1902.04118)]
[[code](https://git.uwaterloo.ca/wise-lab/wise-move)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![[Source](https://arxiv.org/abs/1902.04118).](media/2019_lee.PNG "[Source](https://arxiv.org/abs/1902.04118).")  |
|:--:|
| *[Source](https://arxiv.org/abs/1902.04118).* |

- Some related concepts:
  - `MCTS`, `options framework`, `LTL`, `hierarchical decision making`, `POMDP`
- One related work: The presented approach reminds me the work of [Paxton, C., Raman, V., Hager, G. D., & Kobilarov, M.](https://arxiv.org/abs/1703.07887).
- One term: **"WiseMove"**: the presented **options**-based **modular** safe DRL framework.
  - The _modular_, or _hierarchical_, aspect comes from the [**option framework**](http://www-anw.cs.umass.edu/~barto/courses/cs687/Sutton-Precup-Singh-AIJ99.pdf). Sometimes called **macro-actions**.
  - For more on **Hierarchical RL**, check out [this `thegradient.pub` post](https://thegradient.pub/the-promise-of-hierarchical-reinforcement-learning/).
  - The idea is to decompose the decision by working with **temporal abstracted actions** (e.g. _slow down_, _turn left_) on a high-level (like a **behaviour planner**).
  - Each of these so called _options_ rely on **low-level primitive policies** that implement their manoeuvres (similar to a **geometrical trajectory optimizer**).
- One idea: **LTL** formalism is used to check the validity of high-level decisions.
  - An option is defined by (1) a **underlying primitive policy**, but also by (2) a **initial condition** and (3) a **terminal condition**.
  - For instance, the option `take-over` is available only if a vehicle is on my lane and a second lane exists. The manoeuvre is finished when I arrive in front of the other vehicle.
  - _I like to think of it as another sort of_ **_masking mechanism_**.
  - Here, these conditions are expressed as **hand-crafted rules** in an **LTL**-like syntax.
- One remark: I think we are currently missing open-source simulators that offers OpenAI `gym`-like APIs for training and testing RL approaches for decision making.
  - Several interfaces to `SUMO` have been developed.
  - For instance [@LucasAlegre](https://github.com/LucasAlegre/sumo-rl), [@bstriner](https://github.com/bstriner/gym-traffic/), [@SaloniDash7](https://github.com/SaloniDash7/gym-sumo), [@sycdlcrain](https://github.com/sycdlcrain/gym_sumo) or [FLOW](https://flow-project.github.io/) which looks promising since it keeps being developed.
  - Here, the author of `WiseMove` release an `env` python module (together with `verifier`, `options` and `backends`) that should fulfil this function.
- Another remark: Combining learning [RL] and planning [(MC) tree search] is an idea [I find](https://github.com/chauvinSimon/IV19#combining-learning-and-planning) very promising.
  - Here, the **safest next option** is selected based on the stochastic look-aheads performed by the MCTS (_safety check_).
  - In return, the options effectively **reduce the number of decisions** needed to reach any depth in the tree (_sampling efficiency_).

</details>

Augustin, D., Schucker, J., Tschirner, J., Hofmann, M., & Konigorski, L. [2019].
**"A Simulation-Based Reinforcement Learning Approach for Long-Term Maneuver Planning in Highway Traffic Scenarios"**
[[pdf](https://www.researchgate.net/publication/334745733_A_Simulation-Based_Reinforcement_Learning_Approach_for_Long-Term_Maneuver_Planning_in_Highway_Traffic_Scenarios)]

<details>
  <summary>Click to expand</summary>

- One diagram is better than 100 words:

| ![The term `action` comprises a lateral maneuver decision and a set speed request. [Source](https://www.researchgate.net/publication/334745733_A_Simulation-Based_Reinforcement_Learning_Approach_for_Long-Term_Maneuver_Planning_in_Highway_Traffic_Scenarios).](media/2019_augustin.PNG "The term `action` comprises a lateral maneuver decision and a set speed request. [Source](https://www.researchgate.net/publication/334745733_A_Simulation-Based_Reinforcement_Learning_Approach_for_Long-Term_Maneuver_Planning_in_Highway_Traffic_Scenarios).")  |
|:--:|
| *The term `action` comprises a lateral maneuver decision and a set speed request. [Source](https://www.researchgate.net/publication/334745733_A_Simulation-Based_Reinforcement_Learning_Approach_for_Long-Term_Maneuver_Planning_in_Highway_Traffic_Scenarios).* |

- Some related concepts:
  - `combining learning/planning`, `hierarchical/modular decision making`, `POMDP`, `SUMO`
- One remark: I like the **hierarchy** and **modularity** of the approach.
  - Especially the fact that the `action` stays high-level (`speed desire` and `high-level manoeuvre`), as opposed to `steering angle` and `throttle` commands that are often used in RL.
- One promising tool: [FLOW](https://flow-project.github.io/)
  - `FLOW` is a Python library that interfaces the RL libraries [RLlib](https://ray.readthedocs.io/en/latest/rllib.html) and [rllab](https://github.com/rll/rllab) with `SUMO`. It has been developed and is supported by _UC Berkeley_.
  - It has not been used many times (_because of the lack of Windows support?_). Instead, many research using `SUMO` develop their own interface, which makes comparison and reproduction difficult.
  - A few recent `FLOW`-based works can be mentionned though:
    - "Simulation to scaled city: zero-shot policy transfer for traffic control via autonomous vehicles" by [(Jang et al., 2018)](https://arxiv.org/abs/1812.06120)
    - "Benchmarks for reinforcement learning in mixed-autonomy traffic" by [(Vinitsky et al., 2018)](https://arxiv.org/abs/1812.06120)

</details>

Weingertner, P., Autef, A., & Le Cleac’h, S. [2018].
**"Decision Making Under Uncertainty for Urban Driving"**
[[pdf](https://web.stanford.edu/class/aa228/reports/2018/final100.pdf)]
[[code](https://github.com/PhilippeW83440/ACT)]

<details>
  <summary>Click to expand</summary>

One figure:

| ![Comparing the vanilla POMCP and proposed safe variant of it. [Source](https://web.stanford.edu/class/aa228/reports/2018/final100.pdf).](media/2018_weingertner.PNG "Comparing the vanilla POMCP and proposed safe variant of it. [Source](https://web.stanford.edu/class/aa228/reports/2018/final100.pdf).")  |
|:--:|
| *Comparing the vanilla POMCP and proposed safe variant of it. [Source](https://web.stanford.edu/class/aa228/reports/2018/final100.pdf).* |

- Some related concepts:
  - `POMDP`, `MCTS`, `julia`, `probabilistic risk assessment`, `value iteration`
- One algorithm: [`POMCP`](https://papers.nips.cc/paper/4031-monte-carlo-planning-in-large-pomdps).
  - Presented in 2010, `POMCP` is an extension of the traditional **MCTS algorithm to POMDP**.
  - Together with [`DESPOT`], `POMCP` is a often-used POMDP online solver.
- One term: **"observation class"**.
  - Different extensions of `POMCP` and `DESPOT` have been proposed. In the presented approach, the goal is to work with **continuous observations**, while ensuriung safety.
  - The idea is to **limit the number of observation nodes** in the tree by **grouping observations** based on some **utility function**.
  - This utility function should not to be confused with the **offline-learn _value function_** representing the probability of collision.
  - The safe **clusterization of observations** can be based on _smallest `TTC`_ or _smallest distance_ to other participants.
- One idea: **guide the online graph search** using a **offline methods** to improve safety.
  - This is based on the work of [(Bouton, Karlsson, et al., 2019)](https://arxiv.org/abs/1904.07189), where **offline `VI`** (value iteration) is used to compute `P_collision`(`s`, `a`).
  - This **safety criterion** is then used to limit the **set of safe available actions**.
  - In the presented work, the author reason over the `belief` instead of the `state`.
- Another idea: Use a **Kalman Filter** (instead of some particle filters) for **belief updater**.
- One quote:

> "Using an online method based on sparse sampling may lead to safety issues. Rare events with critical consequences may not be sampled leading to sub-optimal and potentially dangerous decisions."

- One promising tool: [POMDPs.jl](https://github.com/JuliaPOMDP/POMDPs.jl)
  - `POMDPs.jl` is an interface for defining, solving, and simulating `MDPs` and `POMDPs` on discrete and continuous spaces. It has been developed and is supported by a team from _Stanford_.
- Two ideas for future works:
  - In their [repo](https://github.com/PhilippeW83440/ACT), the authors suggest **combining learning** (e.g. model-free RL used as an heuristic and/or for rollout) with **planning** (MCTS), mentioning the success of AlphaGo Zero.
  - Another improvment concerns the **transition model** for the observed vehicles. Instead of `CV` (constant velocity) models, one could assume surrounding vehicles are **following a driver model** (e.g. `IDM`) and the task would be to **infer its hidden parameters**.

</details>

Sierra Gonzalez, D. [2019].
**"Towards Human-Like Prediction and Decision-Making for Automated Vehicles in Highway Scenarios"**
[[pdf](https://tel.archives-ouvertes.fr/tel-02184362/document)]

<details>
  <summary>Click to expand</summary>

Note: the planning part of this thesis takes advantage of the **prediction approaches** and the **driver models** referenced in previous sections.

- In particular, the derived predictive model is used for both the **belief update** (instead of often-used _particle filters_ and _IMM filters_) and as a **generative model** (for forward simulations) of the POMDP.
- The **value estimations** in the tree search are based on the learnt driver model and the long-term prediction methods previously referenced.

Some figures:

| ![Comparison of recent POMDP-based planning modules. [Source](https://tel.archives-ouvertes.fr/tel-02184362/document).](media/2019_sierra_gonzalez_1.PNG "Comparison of recent POMDP-based planning modules. [Source](https://tel.archives-ouvertes.fr/tel-02184362/document).")  |
|:--:|
| *Comparison of recent POMDP-based planning modules. [Source](https://tel.archives-ouvertes.fr/tel-02184362/document).* |

| ![Construction of the tree search with belief updates and model-based rollouts. [Source](https://tel.archives-ouvertes.fr/tel-02184362/document).](media/2019_sierra_gonzalez_2.PNG "Construction of the tree search with belief updates and model-based rollouts. [Source](https://tel.archives-ouvertes.fr/tel-02184362/document).")  |
|:--:|
| *Construction of the tree search with belief updates and model-based rollouts. [Source](https://tel.archives-ouvertes.fr/tel-02184362/document).* |

| ![[Source](https://tel.archives-ouvertes.fr/tel-02184362/document).](media/2019_sierra_gonzalez_4.PNG "[Source](https://tel.archives-ouvertes.fr/tel-02184362/document).")  |
|:--:|
| *[Source](https://tel.archives-ouvertes.fr/tel-02184362/document).* |

- Some related concepts:
  - `MCTS`, `online POMDP`, `POMCP`, `progressive widening`, `SUMO`

- The author targets some **_"human-like tactical planning"_**.
  - The **POMDP** formulation is ideal since it considers uncertainty in the `controls`, `states`, and the `intentions` of the traffic participants.
  - The idea is to include **estimation of intentions** for **long-term anticipation**.

- One idea: about the **rollout policy** used for the construction of the search tree.
  - One option is to use a **random** rollout policy.
  - Here, the previously-derived models are used to **predict approximately the long-term development** of traffic scenes.

- Another idea: adapt the combination of **_model-based_** and **_manoeuvre-estimation-based_** predictions, depending on **how far** the **rollout looks into the future**.

> "As we go **deeper into the history tree** (that is, into the future), the **observed dynamics** of the targets at the root node become **less relevant** and so we **rely increasingly** in the model to **predict the behavior** of the obstacles."

- Other related works:
  - The **Hierarchical architecture** of [(Sonu, E., Sunberg, Z., & Kochenderfer, M. J. (2018). _"Exploiting Hierarchy for Scalable Decision Making in Autonomous Driving"_)](https://www.researchgate.net/publication/328455111_Exploiting_Hierarchy_for_Scalable_Decision_Making_in_Autonomous_Driving).
  - The **`Double Progressive Widening (DPW)`** or `progressive unpruning` of [(Sunberg & Kochenderfer, 2017)](https://arxiv.org/abs/1709.06196) to deal with continuous observations.
  - The general approach of [(Bouton, M., Cosgun, A., & Kochenderfer, M. J. (2017). _"Belief state planning for autonomously navigating urban intersections"_)](https://arxiv.org/abs/1704.04322). The main difference is the substitution of the **Interacting Multiple Model** (`IMM`) with **DBN**-based model to **consider the interactions** between vehicles.

- One quote about the _(relative)_ benefits of POMDP formulations:

> "There have not been significative differences between the decisions taken by the **proposed POMDP planner** and the **reactive SUMO model**. This is due to the fact that neither of those scenes truly required to analyze the **long-term consequences** of a maneuver".

</details>