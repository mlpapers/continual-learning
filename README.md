# Continual Learning

## Learning settings and objectives

- **[Task-, domain- and class-incremental learning](https://arxiv.org/abs/1904.07734)** (2019). Task-incremental evaluation supplies task identity; domain-incremental learning keeps the output task under changing inputs; class-incremental learning distinguishes all seen classes. Compare within matching protocols.

- **[Task-free and online streams](https://arxiv.org/abs/2004.07211)** (2020). Dark Experience Replay studies streams with blurred boundaries and shifting class/domain distributions. Task boundaries, repeated passes and test-time task labels change the problem; “continual” does not specify these privileges.

- **[Retention, transfer and resource use](https://arxiv.org/abs/1810.13166)** (2018). Accuracy, forgetting, forward/backward transfer, memory and computation measure different outcomes. A frozen model may retain old predictions while failing to learn new tasks; retention alone does not establish successful learning.

- **[Plasticity loss](https://arxiv.org/abs/2306.13812)** (2023). Networks can lose the ability to fit new tasks, beyond forgetting old ones. Dohare et al. observe this in long task sequences and test continual backpropagation, which replaces low-utility units; preventing forgetting is a separate objective.

## Parameter importance and functional regularization

- **[Elastic Weight Consolidation](https://arxiv.org/abs/1612.00796)** (2016). Penalizes changes to parameters deemed important by a diagonal Fisher approximation. Preserves a local approximation to earlier solutions; performance depends on task similarity, importance estimation and how penalties accumulate.

- **[Synaptic Intelligence](https://arxiv.org/abs/1703.04200)** (2017). Accumulates each parameter’s contribution to loss reduction along training trajectories. Uses these importance estimates to penalize future movement; differs from EWC’s Fisher estimate at a fitted solution.

- **[Memory Aware Synapses](https://arxiv.org/abs/1711.09601)** (2017). Measures parameter importance through sensitivity of model outputs on unlabeled inputs. Protects frequently relevant functions without requiring labels for importance estimation; the sampled inputs determine what gets protected.

- **[Variational continual learning](https://arxiv.org/abs/1710.10628)** (2017). Uses the previous approximate weight posterior as the next prior; optional coresets revisit selected data. Connects regularization to Bayesian updating; retention depends on approximation quality and model assumptions.

- **[Learning without Forgetting](https://arxiv.org/abs/1606.09282)** (2016). Distills the old model’s outputs on new-task inputs while fitting the new task. Avoids storing old examples; preservation depends on how informative the new inputs are about the old task’s behavior.

## Replay, sampling and gradient constraints

- **[Experience replay](https://arxiv.org/abs/1902.10486)** (2019). Jointly trains on incoming examples and a small episodic buffer. Chaudhry et al. find this simple baseline stronger than several specialized methods in four tested single-pass settings; sampling and buffer allocation are part of the method.

- **[iCaRL](https://arxiv.org/abs/1611.07725)** (2016). Combines exemplar storage, representation distillation and nearest-class-mean prediction. Class means are computed from retained exemplars; a fixed total memory budget means fewer stored examples per class as classes accumulate.

- **[Deep generative replay](https://arxiv.org/abs/1705.08690)** (2017). Samples earlier-task inputs from a learned generator and interleaves them with new data. Removes the need to retain raw examples, but adds generator cost and lets synthesis errors propagate through successive tasks.

- **[Gradient Episodic Memory](https://arxiv.org/abs/1706.08840)** (2017). Projects updates to avoid increasing losses on stored examples from past tasks to first order. Constraints protect the replay memory locally; they do not guarantee unchanged population risk on every old task.

- **[A-GEM](https://arxiv.org/abs/1812.00420)** (2018). Replaces GEM’s per-task gradient constraints with a constraint from an averaged replay gradient. Reduces projection cost while relaxing task-specific protection; evaluates single-pass learning with tuning on separate tasks.

- **[Maximally Interfered Retrieval](https://arxiv.org/abs/1908.04742)** (2019). Selects replay examples whose losses would increase most after a proposed update. Targets imminent interference instead of uniform retrieval; extra scoring work belongs in compute-matched comparisons.

- **[Dark Experience Replay](https://arxiv.org/abs/2004.07211)** (2020). Stores examples with logits produced along the learning trajectory and matches those logits during later training. DER++ also replays labels; it combines rehearsal with functional regularization without requiring task boundaries.

## Class imbalance and representation preservation

- **[Bias Correction](https://arxiv.org/abs/1905.13260)** (2019). Fits a small correction to new-class logits using held-out exemplars. Addresses old/new class imbalance; validation examples consume memory and label budget and must be included when comparing methods.

- **[LUCIR](https://openaccess.thecvf.com/content_CVPR_2019/html/Hou_Learning_a_Unified_Classifier_Incrementally_via_Rebalancing_CVPR_2019_paper.html)** (2019). Combines cosine normalization, a less-forget constraint and interclass separation to rebalance old and new classes. Targets representation drift and classifier bias, rather than treating every accuracy loss as parameter forgetting.

- **[Pooled Outputs Distillation](https://arxiv.org/abs/2004.13513)** (2020). Distills pooled spatial features across network layers and uses multiple proxies per class. Designed for long sequences of small class increments; incremental task size is an important part of its empirical comparison.

## Isolation, capacity and expanding networks

- **[Progressive networks](https://arxiv.org/abs/1606.04671)** (2016). Freezes old task columns and adds a new column with lateral connections. Preserves old pathways by construction and enables feature reuse; parameter count grows with tasks and selecting the correct task pathway matters.

- **[PackNet](https://arxiv.org/abs/1711.05769)** (2017). Iteratively prunes and retrains to reserve weights for successive tasks. Task masks protect allocated parameters; task identification and finite unused capacity constrain this form of no-forgetting.

- **[Hard Attention to the Task](https://arxiv.org/abs/1801.01423)** (2018). Learns task-specific unit masks and restricts later updates using earlier masks. Explicit task routing supports retention; results do not directly transfer to evaluation where task identity is unavailable.

- **[Dynamically Expandable Representation](https://arxiv.org/abs/2103.16788)** (2021). Freezes old feature extractors and adds/prunes new feature dimensions for incoming classes. This DER expands capacity; it is distinct from Dark Experience Replay despite sharing the acronym.

## Continual adaptation of pretrained models

- **[Learning to Prompt](https://arxiv.org/abs/2112.08654)** (2021). Retrieves learnable prompts for a frozen pretrained backbone without test-time task identity. Stores prompt parameters instead of an example buffer; external pretraining supplies representations unavailable to from-scratch baselines.

- **[DualPrompt](https://arxiv.org/abs/2204.04799)** (2022). Combines prompts intended to capture task-shared and task-specific knowledge in a pretrained transformer. Evaluates rehearsal-free class-incremental learning; report pretraining, prompt capacity and routing alongside memory claims.

- **[RanPAC](https://arxiv.org/abs/2307.02251)** (2023). Maps pretrained features through fixed random weights and a nonlinearity, then fits a classifier from class sums and a Gram matrix. Stores statistics instead of examples; optional first-task adaptation must be counted separately.

## Adaptation to changing streams

- **[Adaptive windows for drift: ADWIN](https://www.cs.upc.edu/~Gavalda/papers/adwin06.pdf)** (2006). Shrinks a data window when subwindow means differ significantly; can monitor errors or update learner statistics. Guarantees assume bounded independent observations. Adapting to drift differs from retaining historical tasks.

## Comparative evidence and evaluation

- **[Scenario-dependent findings](https://arxiv.org/abs/1904.07734)** (2019). On split/permuted MNIST, regularization methods struggle when task identity must be inferred, while replay remains effective. This comparison supports specifying the scenario; it does not rank methods across all natural data streams.

- **[Strong-baseline audit](https://arxiv.org/abs/1810.12488)** (2018). Hsu et al. find simple regularization and rehearsal competitive under consistent protocols. Task-label access and evaluation design can dominate claimed method gains; comparisons need matched assumptions, not merely a shared dataset.

- **[Compute-budgeted continual learning](https://openaccess.thecvf.com/content/CVPR2023/html/Prabhu_Computationally_Budgeted_Continual_Learning_What_Does_Matter_CVPR_2023_paper.html)** (2023). With full-history storage and per-step training budgets, simple replay beats costly sampling/distillation in tested ImageNet2K and Continual Google Landmarks streams. Results do not rank learners under a fixed small buffer.

- **[Natural temporal shift: CLEAR](https://datasets-benchmarks-proceedings.neurips.cc/paper_files/paper/2021/hash/2838023a778dfaecdc212708f721b788-Abstract-round2.html)** (2021). Chronological imagery exposes drops from same-period to future-period accuracy. Unsupervised pretraining with a simple classifier beats tested supervised continual learners; pretraining and temporal splits change the comparison.

- **[GDumb](https://www.ecva.net/papers/eccv_2020/papers_ECCV/html/3587_ECCV_2020_paper.php)** (2020). Maintains a class-balanced buffer and trains from scratch on it at evaluation. Its strong results challenge several tested methods, but query-time retraining is not equivalent to an always-ready online learner.

- **[Robust evaluation design](https://arxiv.org/abs/1805.09733)** (2018). Farquhar and Gal show that common setups can favor methods through unrealistic assumptions. Harder datasets alone do not repair a protocol that provides unavailable task information or fails to test retention appropriately.

- **[CLEVA-Compass](https://arxiv.org/abs/2110.03331)** (2021). Records assumptions and evaluation choices across continual-learning studies. Makes differences in data access, task information and resources explicit; a shared label such as class-incremental learning is not a complete comparison specification.

- **[Accuracy through time](https://arxiv.org/abs/1810.13166)** (2018). Report performance after each update stage, transfer and forgetting, plus memory and computation. Match replay bytes, saved logits, optimizer state and growing parameters; a final accuracy can hide poor learning speed or temporary collapse.

## Surveys and software

- **[Continual-learning survey](https://arxiv.org/abs/2302.00487)** (2023). Connects stability–plasticity tradeoffs, generalization and resource efficiency across method families. Useful for reference discovery; individual theoretical and empirical claims should be traced to their original papers.

- **[Avalanche](https://avalanche.continualai.org/)** (documentation). Provides continual-learning benchmarks, training strategies and evaluation tools in PyTorch. Scenario generators and metrics aid reproducibility; the experiment must still state data order, task labels, buffers and tuning access.

## Related topics

[Transfer learning](https://github.com/mlpapers/transfer-learning) · [Meta learning](https://github.com/mlpapers/meta-learning) · [Machine unlearning](https://github.com/mlpapers/machine-unlearning) · [Time series forecasting](https://github.com/mlpapers/time-series-forecasting) · [Evaluation](https://github.com/mlpapers/evaluation) · [Reinforcement learning](https://github.com/mlpapers/reinforcement-learning)
