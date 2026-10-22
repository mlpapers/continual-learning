# Continual Learning

## Learning settings and objectives

- **Task-, domain- and class-incremental learning** - Task-incremental evaluation supplies task identity; domain-incremental learning keeps the output task under changing inputs; class-incremental learning distinguishes all seen classes. Compare within matching protocols. [van de Ven and Tolias 2019](<https://arxiv.org/abs/1904.07734> "Three scenarios for continual learning; arXiv").
- **Task-free and online streams** - Dark Experience Replay studies streams with blurred boundaries and shifting class/domain distributions. Task boundaries, repeated passes and test-time task labels change the problem; "continual" does not specify these privileges. [Buzzega et al. 2020](<https://arxiv.org/abs/2004.07211> "Dark Experience for General Continual Learning: a Strong, Simple Baseline; NeurIPS").
- **Retention, transfer and resource use** - Accuracy, forgetting, forward/backward transfer, memory and computation measure different outcomes. A frozen model may retain old predictions while failing to learn new tasks; retention alone does not establish successful learning. [Díaz-Rodríguez et al. 2018](<https://arxiv.org/abs/1810.13166> "Don&#x27;t forget, there is more than forgetting: new metrics for Continual Learning; arXiv").
- **Plasticity loss** - Networks can lose the ability to fit new tasks, beyond forgetting old ones. Dohare et al. observe this in long task sequences and test continual backpropagation, which replaces low-utility units; preventing forgetting is a separate objective. [Dohare et al. 2023](<https://arxiv.org/abs/2306.13812> "Maintaining Plasticity in Deep Continual Learning; arXiv").

## Parameter importance and functional regularization

- **Elastic Weight Consolidation** - Penalizes changes to parameters deemed important by a diagonal Fisher approximation. Preserves a local approximation to earlier solutions; performance depends on task similarity, importance estimation and how penalties accumulate. [Kirkpatrick et al. 2017](<https://arxiv.org/abs/1612.00796> "Overcoming catastrophic forgetting in neural networks; PNAS").
- **Synaptic Intelligence** - Accumulates each parameter's contribution to loss reduction along training trajectories. Uses these importance estimates to penalize future movement; differs from EWC's Fisher estimate at a fitted solution. [Zenke et al. 2017](<https://arxiv.org/abs/1703.04200> "Continual Learning Through Synaptic Intelligence; ICML").
- **Memory Aware Synapses** - Measures parameter importance through sensitivity of model outputs on unlabeled inputs. Protects frequently relevant functions without requiring labels for importance estimation; the sampled inputs determine what gets protected. [Aljundi et al. 2018](<https://arxiv.org/abs/1711.09601> "Memory Aware Synapses: Learning what (not) to forget; ECCV").
- **Variational continual learning** - Uses the previous approximate weight posterior as the next prior; optional coresets revisit selected data. Connects regularization to Bayesian updating; retention depends on approximation quality and model assumptions. [Nguyen et al. 2018](<https://arxiv.org/abs/1710.10628> "Variational Continual Learning; ICLR").
- **Learning without Forgetting** - Distills the old model's outputs on new-task inputs while fitting the new task. Avoids storing old examples; preservation depends on how informative the new inputs are about the old task's behavior. [Li and Hoiem 2016](<https://arxiv.org/abs/1606.09282> "Learning without Forgetting; ECCV").

## Replay, sampling and gradient constraints

- **Experience replay** - Jointly trains on incoming examples and a small episodic buffer. Chaudhry et al. find this simple baseline stronger than several specialized methods in four tested single-pass settings; sampling and buffer allocation are part of the method. [Chaudhry et al. 2019](<https://arxiv.org/abs/1902.10486> "On Tiny Episodic Memories in Continual Learning; arXiv").
- **iCaRL** - Combines exemplar storage, representation distillation and nearest-class-mean prediction. Class means are computed from retained exemplars; a fixed total memory budget means fewer stored examples per class as classes accumulate. [Rebuffi et al. 2017](<https://arxiv.org/abs/1611.07725> "iCaRL: Incremental Classifier and Representation Learning; CVPR").
- **Deep generative replay** - Samples earlier-task inputs from a learned generator and interleaves them with new data. Removes the need to retain raw examples, but adds generator cost and lets synthesis errors propagate through successive tasks. [Shin et al. 2017](<https://arxiv.org/abs/1705.08690> "Continual Learning with Deep Generative Replay; NeurIPS").
- **Gradient Episodic Memory** - Projects updates to avoid increasing losses on stored examples from past tasks to first order. Constraints protect the replay memory locally; they do not guarantee unchanged population risk on every old task. [Lopez-Paz and Ranzato 2017](<https://arxiv.org/abs/1706.08840> "Gradient Episodic Memory for Continual Learning; NeurIPS").
- **A-GEM** - Replaces GEM's per-task gradient constraints with a constraint from an averaged replay gradient. Reduces projection cost while relaxing task-specific protection; evaluates single-pass learning with tuning on separate tasks. [Chaudhry et al. 2019](<https://arxiv.org/abs/1812.00420> "Efficient Lifelong Learning with A-GEM; ICLR").
- **Maximally Interfered Retrieval** - Selects replay examples whose losses would increase most after a proposed update. Targets imminent interference instead of uniform retrieval; extra scoring work belongs in compute-matched comparisons. [Aljundi et al. 2019](<https://arxiv.org/abs/1908.04742> "Online Continual Learning with Maximally Interfered Retrieval; NeurIPS").
- **Dark Experience Replay** - Stores examples with logits produced along the learning trajectory and matches those logits during later training. DER++ also replays labels; it combines rehearsal with functional regularization without requiring task boundaries. [Buzzega et al. 2020](<https://arxiv.org/abs/2004.07211> "Dark Experience for General Continual Learning: a Strong, Simple Baseline; NeurIPS").

## Class imbalance and representation preservation

- **Bias Correction** - Fits a small correction to new-class logits using held-out exemplars. Addresses old/new class imbalance; validation examples consume memory and label budget and must be included when comparing methods. [Wu et al. 2019](<https://arxiv.org/abs/1905.13260> "Large Scale Incremental Learning; arXiv").
- **LUCIR** - Combines cosine normalization, a less-forget constraint and interclass separation to rebalance old and new classes. Targets representation drift and classifier bias, rather than treating every accuracy loss as parameter forgetting. [Hou et al. 2019](<https://openaccess.thecvf.com/content_CVPR_2019/html/Hou_Learning_a_Unified_Classifier_Incrementally_via_Rebalancing_CVPR_2019_paper.html> "Learning a Unified Classifier Incrementally via Rebalancing; CVPR").
- **Pooled Outputs Distillation** - Distills pooled spatial features across network layers and uses multiple proxies per class. Designed for long sequences of small class increments; incremental task size is an important part of its empirical comparison. [Douillard et al. 2020](<https://arxiv.org/abs/2004.13513> "PODNet: Pooled Outputs Distillation for Small-Tasks Incremental Learning; ECCV").

## Isolation, capacity and expanding networks

- **Progressive networks** - Freezes old task columns and adds a new column with lateral connections. Preserves old pathways by construction and enables feature reuse; parameter count grows with tasks and selecting the correct task pathway matters. [Rusu et al. 2016](<https://arxiv.org/abs/1606.04671> "Progressive Neural Networks; arXiv").
- **PackNet** - Iteratively prunes and retrains to reserve weights for successive tasks. Task masks protect allocated parameters; task identification and finite unused capacity constrain this form of no-forgetting. [Mallya and Lazebnik 2017](<https://arxiv.org/abs/1711.05769> "PackNet: Adding Multiple Tasks to a Single Network by Iterative Pruning; arXiv").
- **Hard Attention to the Task** - Learns task-specific unit masks and restricts later updates using earlier masks. Explicit task routing supports retention; results do not directly transfer to evaluation where task identity is unavailable. [Serrà et al. 2018](<https://arxiv.org/abs/1801.01423> "Overcoming catastrophic forgetting with hard attention to the task; ICML").
- **Dynamically Expandable Representation** - Freezes old feature extractors and adds/prunes new feature dimensions for incoming classes. This DER expands capacity; it is distinct from Dark Experience Replay despite sharing the acronym. [Yan et al. 2021](<https://arxiv.org/abs/2103.16788> "DER: Dynamically Expandable Representation for Class Incremental Learning; CVPR").

## Continual adaptation of pretrained models

- **Learning to Prompt** - Retrieves learnable prompts for a frozen pretrained backbone without test-time task identity. Stores prompt parameters instead of an example buffer; external pretraining supplies representations unavailable to from-scratch baselines. [Wang et al. 2022](<https://arxiv.org/abs/2112.08654> "Learning to Prompt for Continual Learning; CVPR").
- **DualPrompt** - Combines prompts intended to capture task-shared and task-specific knowledge in a pretrained transformer. Evaluates rehearsal-free class-incremental learning; report pretraining, prompt capacity and routing alongside memory claims. [Wang et al. 2022](<https://arxiv.org/abs/2204.04799> "DualPrompt: Complementary Prompting for Rehearsal-free Continual Learning; ECCV").
- **RanPAC** - Maps pretrained features through fixed random weights and a nonlinearity, then fits a classifier from class sums and a Gram matrix. Stores statistics instead of examples; optional first-task adaptation must be counted separately. [McDonnell et al. 2023](<https://arxiv.org/abs/2307.02251> "RanPAC: Random Projections and Pre-trained Models for Continual Learning; NeurIPS").

## Adaptation to changing streams

- **Adaptive windows for drift: ADWIN** - Shrinks a data window when subwindow means differ significantly; can monitor errors or update learner statistics. Guarantees assume bounded independent observations. Adapting to drift differs from retaining historical tasks. [Bifet and Gavaldà 2007](<https://www.cs.upc.edu/~Gavalda/papers/adwin06.pdf> "Learning from Time-Changing Data with Adaptive Windowing; SDM").

## Comparative evidence and evaluation

- **Scenario-dependent findings** - On split/permuted MNIST, regularization methods struggle when task identity must be inferred, while replay remains effective. This comparison supports specifying the scenario; it does not rank methods across all natural data streams. [van de Ven and Tolias 2019](<https://arxiv.org/abs/1904.07734> "Three scenarios for continual learning; arXiv").
- **Strong-baseline audit** - Hsu et al. find simple regularization and rehearsal competitive under consistent protocols. Task-label access and evaluation design can dominate claimed method gains; comparisons need matched assumptions, not merely a shared dataset. [Hsu et al. 2018](<https://arxiv.org/abs/1810.12488> "Re-evaluating Continual Learning Scenarios: A Categorization and Case for Strong Baselines; NeurIPS CL Workshop").
- **Compute-budgeted continual learning** - With full-history storage and per-step training budgets, simple replay beats costly sampling/distillation in tested ImageNet2K and Continual Google Landmarks streams. Results do not rank learners under a fixed small buffer. [Prabhu et al. 2023](<https://openaccess.thecvf.com/content/CVPR2023/html/Prabhu_Computationally_Budgeted_Continual_Learning_What_Does_Matter_CVPR_2023_paper.html> "Computationally Budgeted Continual Learning: What Does Matter?; CVPR").
- **Natural temporal shift: CLEAR** - Chronological imagery exposes drops from same-period to future-period accuracy. Unsupervised pretraining with a simple classifier beats tested supervised continual learners; pretraining and temporal splits change the comparison. [Lin et al. 2021](<https://datasets-benchmarks-proceedings.neurips.cc/paper_files/paper/2021/hash/2838023a778dfaecdc212708f721b788-Abstract-round2.html> "The CLEAR Benchmark: Continual LEArning on Real-World Imagery; NeurIPS").
- **GDumb** - Maintains a class-balanced buffer and trains from scratch on it at evaluation. Its strong results challenge several tested methods, but query-time retraining is not equivalent to an always-ready online learner. [Prabhu et al. 2020](<https://www.ecva.net/papers/eccv_2020/papers_ECCV/html/3587_ECCV_2020_paper.php> "GDumb: A Simple Approach that Questions Our Progress in Continual Learning; ECCV").
- **Robust evaluation design** - Farquhar and Gal show that common setups can favor methods through unrealistic assumptions. Harder datasets alone do not repair a protocol that provides unavailable task information or fails to test retention appropriately. [Farquhar and Gal 2018](<https://arxiv.org/abs/1805.09733> "Towards Robust Evaluations of Continual Learning; arXiv").
- **CLEVA-Compass** - Records assumptions and evaluation choices across continual-learning studies. Makes differences in data access, task information and resources explicit; a shared label such as class-incremental learning is not a complete comparison specification. [Mundt et al. 2022](<https://arxiv.org/abs/2110.03331> "CLEVA-Compass: A Continual Learning EValuation Assessment Compass to Promote Research Transparency and Comparability; ICLR").
- **Accuracy through time** - Report performance after each update stage, transfer and forgetting, plus memory and computation. Match replay bytes, saved logits, optimizer state and growing parameters; a final accuracy can hide poor learning speed or temporary collapse. [Díaz-Rodríguez et al. 2018](<https://arxiv.org/abs/1810.13166> "Don&#x27;t forget, there is more than forgetting: new metrics for Continual Learning; arXiv").

<a id="surveys-and-software"></a>

## Surveys

- **Continual-learning survey** - Connects stability-plasticity tradeoffs, generalization and resource efficiency across method families. Useful for reference discovery; individual theoretical and empirical claims should be traced to their original papers. [Wang et al. 2023](<https://arxiv.org/abs/2302.00487> "A Comprehensive Survey of Continual Learning: Theory, Method and Application; arXiv").

## Software

- **Avalanche** - Provides continual-learning benchmarks, training strategies and evaluation tools in PyTorch. Scenario generators and metrics aid reproducibility; the experiment must still state data order, task labels, buffers and tuning access. [docs](<https://avalanche.continualai.org/>).

## Related topics

- [Transfer learning](https://mlpapers.org/transfer-learning/)
- [Meta learning](https://mlpapers.org/meta-learning/)
- [Machine unlearning](https://mlpapers.org/machine-unlearning/)
- [Time series forecasting](https://mlpapers.org/time-series-forecasting/)
- [Evaluation](https://mlpapers.org/evaluation/)
- [Reinforcement learning](https://mlpapers.org/reinforcement-learning/)
