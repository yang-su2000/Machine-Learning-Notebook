# Source

- [A Survey of Large Language Models](https://github.com/RUCAIBox/LLMSurvey)

# Intro

**Statistical Language Models (SLMs)**

- based on statistical learning from 1990s
- main idea: build the word prediction model based on the *Markov assumption*
	- predict the next word based on the most recent context
- *n-gram* LMs: SLM with a fixed context length n, e.g. *bigram*, *trigram* LMs
- issue: curse of dimensionality, i.e. exponential number of transition % need to be estimated
	- smoothing strategies: back-off estimation, Good-Turing estimation

**Neural Languge Models (NLMs)**

- based on distributed representation of words
- model context representation by aggregating related distributed word vectors
- *word2vec*: a simplified shallow neural network for learning distributed word representaions

**Pre-trained Language Models (PLMs)**

- *ELMo*: capture context-aware word representations
	- first pre-train a biLSTM network instead of learning fixed word representations
	- then fine-tune the network according to specific downstream tasks
- *BERT*: pre-trained bidirectional language models
	- transformer, self-attention
	- "pre-train, fine-tuning" learning paradigm
	- followed studies: GPT-2, BART, etc.

**Large Language Models (LLMs)**

- scaling law: scaling PLM leads to "emergent abilities"
	- e.g. 175B GPT-3, 540B PaLM
- in-context learning: few-shot tasks solving ability from GPT-3 that GPT-2 does not have
- difference of PLMs and LLMs
	- emergent ability from scaling
	- prompting opposed to fine-tuning
	- extensive engineering work in large-scale data processing and distributed learning
- *ChatGPT*, *GPT-4*, etc.
- challenge
	- why emergent abilities occur
	- difficult to train for academia, mainly industry
	- hard to align LLMs with human values or perferences

- paper main focus
	- pre-traning of LLMs
	- effective fine tuning of LLMs
	- usability of LLMs for various downstream tasks
	- evaluation of LLMs

### Overview

- what are LLMs
	- usually larger than 10 B
	- built upon transformer architecture with multi-head attention layers
	- largely scaled model size, pre-training data, and total compute (orders of magnification than PLMs)
- emergent abilities of LLMs
	- "the abilities that are not present in small models but arise in large models"
	- analogy: *phase transition* in physics
	- *in-context learning*
	- *instruction following*
	- *step-by-step reasoning*
- key techniques for LLMs
	- scaling: on all of model size, data size, and total compute (not just one), with high data quality - so data collection and cleaning strategies are very important
	- training: distributed parallel algorithms, e.g. DeepSpeed, Megatron-LM; optimization trickes, e.g. restart with training loss spike, mixed-precision training, predict performance of LLMs with much smaller models by GPT-4
	- ability eliciting: use prompt to design suitable task instruction or specific in-context strategies, e.g. *chain-of-thought prompting*
	- alignment tuning: helpful, honest and harmless rather than toxic, biased, harmful content from low-quality data, e.g. InstructGPT with RLHF (*reinforcement learning with human feedback*) - human in the loop with elborately designed labeling strategies
	- tools manipulation: LLMs perform less well on tasks that are not best formed or expressed in the text (i.e. numerical computation), and they are limited to non up-to-date data - solve: employ external tools, e.g. ChatGPT Plugins

### Resources of LLMs

- 10B ~ 100B open sourced LLMs
	- mT5, T0, GPT-NeoX-20B, CodeGen, UL2, Flan-T5, mT0, PanGu-a
	- *Flan-T5* (11B) explores instruction tuning from 3 aspects
		- increasing number of tasks
		- scaling model size
		- fine-tuning with chain-of-thought prompting data
	- *CodeGen* (11B) introduces new benchmark MTPB for multi-turn program synthesis, composed by 115 expert-generated problems
	- *mT0* (13B) is fine tuned on multilingual tasks with multilingual prompts
	- *LLaMA* (65B) is superior in instruction following, using 2048 A100-80G GPUs
- 100B+ open sourced LLMs
	- OPT, OPT-IML, BLOOM, BLOOMZ, GLM, Galactica
	- *BLOOM* (176B) and BLOOMZ (176B) researches in cross-lingual generalization
	- instruction fine-tuned: OPT-IML, Galactica, GLM
- public APIs (all from OpenAI...)
	- GPT-3 series fine-tunable: ada, babbage (GPT-3, 1B), curie (GPT-3, 6.7B), davinci (GPT-3, 175B, most powerful)
	- GPT-3 series non fine-tunable: text-ada-001, text-babbage-001, text-curie-001
	- Codex series: code-cushman-001 (multilingual version of Codex), code-davinci-002
	- GPT-3.5 series: code-davinci-002, text-davinci-002/3, gpt-3.5-turbo-0301 (ChatGPT)
	- GPT-4 series: gpt-4, gpt-4-0314, gpt-4-32k, gpt-4-32k-0314
- commonly used corpora
	- Book: *BookCorpus* (common in small-scale models, i.e. GPT-1/2), *Project Gutenberg* (common in large-scale models, i.e. LLaMA), Books1+2 (GPT-3 used, private)
	- *CommonCrawl*: one of the largest open-source web crawling database, too big and has low-quality info, usually need preprocessing: 4 filtered subsets: C4, CC-Stories, CC-News (76G), RealNews (120G)
		- *C4* (Colossal Clean Crawled Corpus): has 5 variants: en (806G), en.noclean (6T), realnewslike (36G), web-textlike (17G), multilingual (38T)
		- C4-en is used in T5, LaMDA, Gopher, UL2
		- C4-multilingual (mC4) is used in mT5
		- CC-Stories original source not available, CC-Stories-R (reproduced) is public
	- Reddit Links: *WebText* includes highly upvoted reddit posts as they are likely more useful and high-quality but not public, *OpenWebText* is public, *PushShift.io* consists of hidstorical data from Reddit and real-time updated
	- *Wikipedia*: the English-only filtered version is used in most LLMs
	- Code: *GitHub* and *StackOverflow*, and Google *BigQuery dataset* and a subset of it BIGQUERY used by CodeGen
	- Others: the *Pile* (800G) is a multi-source dataset including books, websites, codes, scientific papers, and social media platforms, *ROOTS* consists of various smaller datasets (1.61T total) with 50 languages, used for BLOOM
- pre-traning corpora of 3 representative LLMs
	- GPT-3: 300B tokens including CommonCrawl, WebText2, Books1+2, Wikipedia
	- PaLM: 780B tokens including social media conversations, filtered webpages, books, GitHub, multilingual Wikipedia, and news
	- LLaMA: 1T tokens for LLaMA 6B, 1.4T for LLaMA 32/65B, including CommonCrawl, C4, GitHub, Wikipedia, books, ArXiv, and StackExchange
- library
	- traditional ones: PyTorch, TensorFlow, MXNet, PaddlePaddle, MindSpore, OneFlow
	- *Transformers*: transformer oriented model collection by huggingface
	- *DeepSpeed*: deep learning optimization library by Microsoft
		- distributed training: memory optimization (ZeRO technique), gradient checkpointing, pipeline parallelism
		- APIs for LLM fine-tuning and evaluation
	- *Megatron-LM*: deep learning optimization library by NVIDIA
		- distributed training: model and data prallelism, mixed-precision training, FlashAttention, gradient checkpointing
	- *JAX*: high-performace ML library by Google Brain
		- just-in-time compilation acceleration, automatic batching
	- *Colossal-AI*: on top of JAX, supports mixed-precision training and parallelsim
	- *BMTrain*: efficient library by OpenBMB
	- *FastMoE*: specialized library for MoE models, enables data and model parallelism

### Pre-Training

**Data Collection**

- data source
	- general data: large, diverse, public
		- webpages: CommonCrawl, high quality Wikipedia, low qualty spam mail
		- conversation text: PushShift.io Reddit corpus, process to tree structure (response-response link), side effect: declarative instructions and direct interrogatives are wrongly perceived as BOS
		- books: Books3, Bookcorpus2, formal and long-term dependency, narrative and coherent
	- specialized data: specific task-solving abilities
		- multilingual text: BLOOM and PaLM has 46 and 122 languages, beneficial to translation, multilingual summarization and QA
		- scientific text: arXiv, scientific textbooks, math webpages, the complexity of scientific symbol (math, biology) needs specific tokenization and preprocessing
		- code: StackExchange, GitHub, long-range dependencies and accurate execution logic, complex reasoning abilities (chain-of-thought)
	- data source for current LLMs
		- T5 11B: 100% webpages
		- mT5 13B: 100% webpages
		- LLaMA 65B: 87% webpages, 7% code, 5% books, 3% scentific
		- GPT-3 175B: 64% webpages, 17% code, 13% books, 5% conversation
		- MT-NLG 530B: 50% webpages, 11% code, 23% books, 9% scientific
		- Gopher 280B: 57% webpages, 7% code, 35% books
		- Chinchilla 70B: 55% webpages, 6% code, 39% books
		- GLaM 1200B: 48% webpages, 30% books, 22% conversation
		- PaLM 540B: 42% webpages, 10% code, 14% book, 35% conversation
		- LaMDA 137B: 38% webpages, 13% code, 50% conversation
		- Galactica 120B: 10% webpages, 7% coe, 83% scentific
		- GPT-NeoX 20B: 32% webpages, 8% code, 16% books, 4% conversation
		- CodeGen 16B: 19% webpages, 46% code, 9% books, 23% scentific
		- AlphaCode 41B: 100% code
- data preprocessing
	- *quality filtering*: remove low-quality data
		- classifier based: a binary classifier trained on well-curated data as positive, other as negative, predict the score to measure the quality of each data sample - this may result in removing high-quality text in some languages and lead to biased corpus
		- heuristic based (used in BLOOM, Gopher)
			1. language filtering: remove if non-multilingual, etc.
			2. metric filtering: use evaluation metric, i.e. perplexity to detect and remove unnatural sentences
			3. statistic filtering: punctuation distribution, symbol-to-word ratio, sentence length, etc. can measure data quality
			4. keyword filtering: remove noisy and unuseful elements, i.e. HTML, links, offensive words
	- *de-duplication*: duplicating data would reduce diversity and cause unstable training - do the opposite - remove similar data!
		- sentence-level: remove sentence with repeated words and phrases
		- document-level: remove by overlap ratio (word, n-gram overlap) between documents
		- dataset-level: remove duplicate text from training set that happens in evaluation set
	- *privacy redaction*: web sources and user-generated content has sensitive and personal info - privacy risks - remove PII (*personally identifiable information*)
		- rule-based: keyword spotting, i.e. names, addresses, phone numbers
		- de-duplication also helps this step
	- *tokenization*: customized tokenization helps compared to use existing ones from other models
		- popular: *SentencePiece* with BPE (*Byte Pair Encoding*) ensures that information after tokenization is lossless
		- but normalization techinique in BPE such as *NFKC* may degrade tokenization performance
- effect of pre-training data
	- distribution of mixture of sources is important: more on C4, better on C4, but worse on other data
	- *scaling law*: model size positively correlated with data size, should balance them, more data size and high qualities can make smaller model become closer to larger model performance
	- quantity and quality are both important: duplication of data may result in *double descent* or overwhelm the training process, and degrades model's ability to copy from context - further affect in-context learning ability

**Architecture**

- transformer
	- encoder-decoder: the vanilla transformer
		- e.g. (Flan-)T5, BART
		- encoder: MHA (multi-head self-attention) to get latent input embedding
		- decoder: cross-attention on encoder output and autoregressively generate target sequence
	- casual decoder: one-directional attention mask to hide input to see its future
		- e.g. GPT-series and a whole bunch of others
	- prefix decoder: bi-directional attention to prefix tokens, one-direction attention to other tokens, usually, train causal decoder from scratch, then switch to prefix decoder to accelerate convergence
		- e.g. U-PaLM (from casual decoder PaLM), GLM 130B
	- MoE (mixture of experts) scaling: a subset of network weights for each input are sparsely activated
		- e.g. Switch Transformer, GLaM
- (layer) normalization
	- where to put LN is important, most LLMs use *Pre-LN* for stable training but decreased performance
	- do not put LN after input embedding, it drops performance
	- *Sandwich-LN*: [x + LN(F(LN(x)))] Pre-LN + one more LN before residual, but sometimes cause training instability
	- *RMS Norm*: LN with mean = 0
	- *DeepNorm*: [LN(ax + F(x))] multiply a in [0, 1] to x
- activation
	- many LLMs use GeLU, some uses SwiGLU, GeGLU, and ReLU
	- *GeLU*
		- combine dropout, zoneout (maintain prev values), and ReLU
		- weight input by percentile rather than gates
		- f(x) = x * guassian cdf(x)
		- f'(x) = (1 + x) * guassian cdf(x)
		- good in NLP, CV and speech
	- *Swich*
		- f(x) = x * sigmoid(x)
		- f'(x) = sigmoid(x) + f(x) * (1 - sigmoid(x))
	- *GeGLU*, *SwiGLU*
		- GeLU(input * W) * (input * V), Swich(input * W) * (input * V)
		- V is learnable
- position embeddings
	- transformer is *permutation equivariant*, position embeddings are used to inject absolute or relative position info
	- absolute embedding: vanilla transformer
	- relative embedding: generated by the offset diff of key and query, hence can extend to longer sequence (extrapolation)
	- *ALiBi*: penalty on distance of key and query, higher the longer distance, has strong extrapolation capacity
	- *RoPE*: add a rotational score calculated based on key, query and their distance, also good on longer sequence
- attention and bias
	- *Sparse Attention*: factorized attention in GPT-3
	- *Flash Attention*: attention optimized GPU memory
	- fewer bias in PaLM and Galactica - more stable training
- pre-training tasks
	- *language modeling* (LM): i.e. next token prediction
		- loss(x) = sum(i=1->n: log prob(xi | x1, x2, ..., xi-1))
	- *prefix language modeling*: the prefix tokens are not used in loss computing, with same number of total tokens, this is worse than lanuage modeling (as fewer tokens are involved in pre-training)
	- *denoising autoencoding* (DAE): input are corputed text with randomly replaced spans, model is trained to recover the spans
		- loss(x) = log prob(x_span | x - x_span + x_random)
		- only T5 and GLM use this so far as it is quite complex
		- they are trained to recover the spans autoregressively
- discussion
	- train with LM objective and causal decoder architecture gives model best zero and few-shot generalization capacity, instruction and alignment tuning later further enhance it further
	- scaling model size, dataset size, and total computation greatly improve causal decoders performance
	- more detailed investigation on encoder-decoder model is lacking

**Model Training**

- optimization setting
	- batch training: usually 8196 sentences, 1.6M tokens
		- GPT-3, PaLM dynamically increase batch size during training until reaching 1M scale (32K -> 3.2M for GPT-3), this stabilizes training
	- learning rate: usually warm-up and decay
		- in the initial 0.1% -> 0.5% training steps, linear warm-up schedule to gradually increase lr to a maximum of 5e-5 -> 1e-4 (6e-5 for GPT-3)
		- then cosine decay to gradually decrease lr to 10% of its maximum value until loss convergence
	- optimizer: usually Adam and AdamW, based on adaptive estimate of lower-order moments for first-order gradient-based optimization
		- beta1 = 0.9, beta2 = 0.95, eps = 1e-8
		- Adafactor (variant of Adam) is also used in PaLM and T5, designed to conserve GPU memory
	- stabilizing the training: weight decay to 0.1, gradient clipping to 1.0
		- loss spike: PaLM and OPT restarts training from an earlier checkpoint before spike and skip over the data that may caused the issue
		- abnormal gradients of embedding layer leads to spike: GLM shrink the embedding layer gradient to alleivate it
- scalable training techniques
	- two headache: increasing training throughput, fit large batch into GPU memory
	- *3D Parallelism*: data, pipeline and tensor parallelism
	- data parallelism
		- replicates model parameters, gradients and optimizer states across multiple GPUs
		- but distributes the whole training data separately into different GPUs
		- in each gradient update for a batch, sum up all gradients from different GPUs
		- implemented in most libraries like PyTorch, TensorFlow
	- pipeline parallelism
		- distributes different layers of LLM into different GPUs
		- but a naive way to implement this results in low GPU utilization rate - each GPU has to wait for previous one to complete the layer computation, leading to *bubbles overhead*
		- *GPipe* and *PipeDream* propose techniques of padding multiple batches of data and asynchronous gradient update to improve pipeline efficiency
	- tensor parallelism
		- decompose the parameter matrix of LLMs to different GPUs
		- Y = X * W = [X * W1, X * W2] where W is split by column
		- place W1, W2 on different GPUs, then matrix multiply is invoked at two GPUs in parallel
		- the final Y can be computed by combining the outputs from two GPUs
		- *Megatron-LM* and *Colossal-AI* implement this and extends to higher-dim tensors, the latter also has sequence parallelism (todo)
	- *ZeRO* from DeepSpeed library
		- focuses on memory redundancy issue in data parallelism
		- no need to store all model parameters, gradients and optimizer states on each GPU
		- proposes...
			1. optimizer state partitioning (no cross-GPU overhead)
			2. gradient partitioning (no cross-GPU overhead)
			3. parameter partitioning (50% cross-GPU overhead)
		- saves memory proportional to # of GPUs
		- PyTorch has a similar technique to ZeRO called *FSDP*
	- mixed precision training
		- fp16 may lead to loss of accuracy
		- bf16 (*Brain Floating Point*) allocates more exponent bits, fewer significant bits than fp16, generally better in accuracy in pre-training than fp16
		- but fp16/bf16 can only be on specific hardwares (e.g. A100)
- overall training suggestion
	- balance 3D parallelism and use them together
		- BLOOM on 384 A100 uses 8-way data, 4-way tensor, and 12-way pipeline parallelism
	- use ZeRO, FSDP, and activation recomputation (todo) to reduce memory redundancy
	- *predictable scaling* is useful to predict model performance ahead of time, introduced by GPT-4
	- quantization techniques are important to increase inference speed: reduce both time and space cost with some loss in performance - INT8 is popular, INT4 is used by some aggressive models

### Adaptation Tuning

- instruction tuning: enhance LLM abilities, alignment tuning: align behavior of LLM with human value or preferences

**Instruction Tuning**

- tasks: xP3 (81M), OIG (43M), MVPCorpus (41M), etc.
- main usage: fine-tune LLMs on a collection of formatted instances in natural language
- formatted instance construction
	- a formatted instance = a task description (*instruction*) + input-output pair + (optional) small number of demonstrations
	- instruction is crucial in LLM task generalization ability, fine-tune without it gives very bad performance
	- *PromptSource*: crowd-sourcing task description sharing platform
	- formatting existing dataset: general proposed instruction, or specially designed instruction
	- formatting human needs: to solve lack of instruction diversity or mismatched human needed instruction, InstructGPT took people's queries as task descriptions, human labelers are indentedly asked to write real-life tasks in various formats, then another group of labelers answer them as output
	- key factors for instance construction
		- scaling the instructions (number of tasks): also increase task diversity in length, structure, creativity; also increase number of instances per task (but not too much like 1mil, would overfit)
		- formatting design: good task description and demonstration are crucial, also alleviate model sensitivity to instruction engineering, but putting [things to avoid, reasons, suggestions, etc.] would have few or even bad effect to model performance; to do step-by-step reasoning, fine-tune with CoT + non-CoT leads to very good reasoning performance and other tasks like analysis and QAs
	- TLDR
		- more diversity of instructions > more instances
		- human-need tasks from labelers is important
		- but since human-need instances lack guidelines, it may feel heuristic
		- can reuse formatted dataset or auto-constructed instructions using existing LLMs to reduce human efforts
- instruction tuning strategies
	- different from supervised pre-training in: 1) training objective (seq2seq loss), 2) optimization config (smaller batch size and lr), 3) special attention in practice, also:
	- balancing data distribution: balance each task is important
		- *examples-proportional mixing* strategy: combine all datasets then sample each instance equally, but increase sampling ratio for high-quality collections (FLAN, P3)
		- *maximum cap* (i.e. 10-100k): control max number of examples a dataset can contain in instruction tuning - prevent larger datasett from overwhelming the entire distribution
	- combining instruction tuning and pre-training
		- *OPT-IML* addes pre-training data in instruction tuning - like regularization for tuning
		- *GLM-130B*, *Galactica* train from scratch with mixed these two datasets - multi-task learning
- effect of instruction tuning
	- performance improvement
		- models from 77M to 540B all benefit from instruction tuning, but better improvement as model is bigger
		- instruction-tuned small models even better than large models un-tuned
		- different model architectures, pre-training objectives, model adaptation methods all benefit from it too
		- more efficient than pre-training as instruction tuning data size is much smaller than pre-training ones
	- task generalization
		- or *emergent ability*, model begins to follow human instructions on unseen tasks without demonstrations
		- instruction tuning also alleviate model weakness on repetitive generation, complementing the input but not accomplishing the task
		- *BLOOMZ-P3* instruction tuned on BLOOM shows that it also generalizes to tasks across languages
		- and using English-only instructions can produce good results on multilingual tasks - helps instruction dataset preparation and engineering efforts

**Alignment Tuning**

- background
	- unintended behaviors: fabricating false info, inaccurate objectives
	- and harmful, misleading, biased experssions
	- NTP (next token prediction) or instruction tuning lacks these
	- need alignment to human preference
	- alignment come with performance drop: *alignment tax*
- alignment criteria repsentatives
	- helpfulness
		- LLM should show a clear attempt to assit users in concise and efficient manner
		- LLM should show additional info through pertinent inquiries, suitable level of sensitivity, perceptiveness, and prudence
		- hard to define and measure the intention of users
	- honesty
		- LLM should show accurate content, not faking info
		- LLM should show appropriate level of uncertainty to avoid misrepresentation of info, i.e. LLM should *know unknowns*
		- honesty is more objective and could be further developed
	- harmlessness
		- LLM should not be offensive or discriminatory
		- LLM should detect malicious purposed requests, i.e. politely refuse when asked dangerous actions
		- *what behaviors* are harmful and *what extent* varies from user groups
	- summary
		- these criteria are subjective, hard to optimize
		- *red teaming* use manual or automated means to probe LLMs adversarially to generate harmful outputs and update LLMs to prevent them
- collect human feedback
	- human labeler selection
		- they need to have qualified level of education and excellent proficiency in English
		- still has mismatch to researchers' intention
		- InstructGPT conducts a screening between researchers and labelers, and filter labelers based on most agreements to researchers on a small amount of data - best among them are *super raters* that label later stage of LLMs as well
		- during annotation, detailed instructions and instant guidance to labelers can further regulate the result
	- human feedback collection
		- ranking-based collection: different labelers have different preferenes -> *ELO Rating* gives a preference ranking
		- question-based collection: labelers can provide detailed feedback while answering certain questions -> WebGPT labelers answer questions about whether retrieved documents are useful for answering the given input
		- rule-based collection: Sparrow uses a series of rules to test whether model responses meet the alignment criteria (helpful, harmless, etc.), then we get two types of feedback
			- response preference feedback by comparing model output in pairs
			- rule violation feedback by collecting labeler assessment score of whether model output violates the alignment criteria
			- GPT-4 uses a set of zero-shot (self-generated) classifiers to auto-determine if model output violate a set of human-written rules
- RLHF (Reinforcemnet Learning from Human Feedback)
	- background: RL PPO (Proximal Policy Optimization) algorithm
	- RLHF system
		- pre-trained LM: the LLM we have pre-trained (175B GPT-3 for InstructGPT, 280B Gopher for GopherCite)
		- reward model: a fine-tuned LM or LM trained with human preference data (usually small, gives scalar output, 6B GPT-3 / 7B Gopher)
		- optimize pre-trained LM with RL alignment
	- key steps in RLHF
		- supervised fine-tuning
			- pre-trained LM are provided a collection of input prompts (instruction) and desired output for its fine-tuning
			- ensure the diversity of task while providing specific tasks
		- reward model training
			- use the fine-tuned LM to generate a certain number of output using sampled prompts from either supervised data or human prompt
			- let human labelers to annotate perference for these outputs by ranking - which reduce incosistency -> then we train reward model to predict this ranking
		- RL fine-tuning
			- the fine-tuned LM acts as the *policy* in RL that takes input as a prompt and return output text
			- the *action space* of the policy is all vocab of the LM
			- the *state* is characterized by the currently generated token sequence
			- the *reward* is generated by the RM
		- RL PPO
			- to avoid deviating too much from the initial (before tuned) LM, a penalty term is incorported into the reward function
			- InstructGPT optimizes the LM against RM with PPO algorithm
			- for each input prpmpt, calculate the KL divergence of the output from the current (supervised fine-tuned + RL fine-tuned) model to the initial (un-tuned) LM as the penalty
		- reward model and RL fine-tune can be iterated multiple turns for better alignment

### Utilization

- prompt formulation
- demonstration design
	- demonstration selection
		- heruistic approaches
		- LLM-based approaches
	- demonstration format
	- demonstration order
- underlying mechanism
	- how pre-training affects ICL
	- how LLMs perform ICL
- in-context learning with CoT
	- few-shot CoT
		- CoT prompt design
		- enhanced CoT strategies
	- zero-shot CoT
- further discussion on CoT
	- when CoT works for LLMs
	- why LLMs can perform CoT reasoning
		- the source of CoT ability
		- the effect of prompting components

### Evaluation

**Task Evaluation**

- language generation
	- language modeling
	- conditional text generation
	- code synthesis
	- major issues
		- controllable generation
		- specialized generation
- knowledge utilization
	- closed-book QA
	- open-book QA
	- knowledge completion
	- major issues
		- hallucination
		- knowledge recency
- complex reasoning
	- knowledge reasoning
	- symbolic reasoning
	- mathematical reasoning
	- major issues
		- inconsistency
		- numerical computation

**Advanced Ability Evaluation**

- human alignment
- interaction with external environment
- tool manipulation

**Benchmarks**

- evaluation benchmarks
	- MMLU
	- BIG-bench
	- HELM
- general analysis on LLM
	- mastery
	- robustness
- specialized analysis on LLM
	- healthcare
	- education
	- law

### Conclusion and Future Directions

- theory and principle
- model architecture
- model training
- model utilization
- safety and alignment
- application and ecosystem
