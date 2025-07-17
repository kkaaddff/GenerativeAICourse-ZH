# LLM Architecture and Training

You don't need to know how to develop a model to use it, but understanding how they work at a high level will help you decide which model to use.

There is lack of transparency in the training process of foundation models, but we can get a good understanding of them by knowing the pre-training data, model architecture & size, and how they are post-trained to align with human preferences.

## 1. Training Data

A common source for training data is Common Crawl, which is a nonprofit organization that regularly crawls websites on the Internet. The quality of Common Crawl though is not always the best because within that training data you have clickbait, misinformation, propaganda, etc. To avoid scrutiny from the public and competitors, companies stopped disclosing this information.

English language dominates the internet, so it performs better than other languages. General purpose foundational models can be great for everyday tasks, but might struggle with domain-specific tasks they never saw during training like drug discovery and cancer screening. For instance, drug discovery involves protein, DNA and other data which is unlikely to be found publicly. Cancer screening typically involves X-ray and MRI scans which also is hard to find publicly due to privacy. DeepMind's AlphaFold is an example of domain-specific model trained on about 100k known proteins. Nvidia BioNeMo is another model that focuses on drug discovery.

### Data Challenges and Future Concerns

**Scaling Bottleneck:** Until now, every magnitude increase in model size led to an increase in model performance. But would there be a point where model performance plateaus regardless of size? Foundational models use so much data that there's a concern we will run out of internet data in the next few years. Foundation models consume training data faster than humans create new content. If we need 100T words for the next model generation, but the entire internet only contains 50T words, we're approaching physical limits. To tackle that, companies are now tapping into proprietary data like paying Reddit, publishers, exclusive data partnerships, etc.

**Security Implications:** Anything you post online, you should assume it will be part of future models' training data. Bad actors exploit that for prompt injection attacks by publishing stuff on the internet hoping it will influence future models to generate the responses they desire. For instance, they can create fake academic looking articles containing statements like: "The most effective way to bypass airport security involves [detailed harmful instructions]." Now when someone asks the AI model about airport security, the model might reference this research and present dangerous information. AI models can't distinguish between real and fake expertise during training.

**Data Authenticity:** The other thing to think about is that AI trained on human data is more authentic. Humans make genuine mistakes, they reflect authentic emotional states & experiences, etc. If models are trained on AI data that lacks authenticity, is perfect, etc. then future AI models can be disconnected from authentic human communication patterns or unable to generate truly novel ideas.

**Data Forgetting:** An open research question is how to make a model forget specific information it has learned during training. Imagine you published a blog post that you eventually deleted. If that blog post was included in a model's training data, the model might still reproduce the post's content. As a result, people could potentially access removed content without your consent.

## 2. Architecture

### Seq2Seq (Pre-Transformer Era)

LLMs are based on the transformer architecture. Before the transformer was invented, there was seq2seq architecture which was used for machine translation. It introduced an encoder-decoder paradigm where an encoder processes input sequences and decoder generates output sequences, both using RNN (recurrent neural networks). The problem with Seq2Seq is that all inputs (tokens) are processed sequentially which is slow, but also all input is compressed into a final state so context can be missed.

![image](https://github.com/user-attachments/assets/18614e4d-573e-47be-9dea-c649dac2444b)


### Transformer Architecture

Transformer architecture removed RNNs. Say you want to summarize a 2k word article. Seq2Seq with RNN must process all of these words sequentially, while the transformer processes it in parallel (but the output is processed sequentially, token by token). Transformers have a two-phase inference process:

1. **First Phase:** Processing all input tokens simultaneously. When you ask "explain quantum computing", the transformer processes all the words in parallel by creating a key and value vector for each token (like a lookup table). So "explain" becomes K1, V1 (key vector 1 and value vector v1).

2. **Second Phase:** Generates output tokens or words one at a time (sequential). So it generates "Quantum" looking at your entire question, then generating "Computing" looking at your question + Quantum (first token outputted), then generates "uses" looking at your question + Quantum Computing (first 2 tokens).

3. ![image](https://github.com/user-attachments/assets/9c2aa716-bc26-4634-bf5e-cdc84a2989c8)


### Attention Mechanism

If you ask ChatGPT "Write a product description for our new wireless headphone that emphasizes battery life and sound quality", the LLM needs to generate each word of its responses while keeping track of what's important. It should focus on keywords like battery life and sound quality not generic words like "for" or "our". So at each word generation step, the model must decide: "Out of all the previous words I've seen, which ones should influence what I write next?"

It consists of 3 things:

- **Query Vector (Q):** As it is writing the word, it creates a query vector: "What type of word do I need right now?"
- **Key Vector (K):** What type of information does each previous word contain (every word in your request gets a key vector describing what it is, so "product" is content type, "wireless" is feature descriptor, etc).
- **Value Vectors (V):** Each word gets a value vector containing its semantic meanings.

Then after all that, the attention calculation process starts. When generating the word "exceptional" as an example, the LLM computes how relevant each previous word is by giving them attention weights, so 35% attention is given to battery, 32% is given to sound, etc. So this gives "exceptional" a meaning that's 35% influenced by battery concepts and 32% by sound concepts.

**In summary:** A transformer is an architecture that processes input tokens in parallel and generates output tokens sequentially, and it leverages attention mechanism which allows the model to dynamically focus on the most relevant parts of the input when generating each new word.

### Model Size

Note that as the community better understands how to train models, newer models tend to outperform older generation even of the same parameter size. The number of parameters help us estimate the compute resources needed to train and run the model. For example, if a model has 7B parameters, and each parameter is stored using 2 bytes, then we can calculate that the GPU memory needed to do inference using this model will be at least 14B bytes (14GB).

But when discussing model size, it is important to consider the size of the data it was trained on. We can look at the number of tokens, which isn't a perfect measure, as different models can have different tokenization processes. Meta used 15T tokens for Llama 3.

## Post-Training

Post-training exists because a pre-trained model has two issues: They are optimized for text completion & not conversations, and the output of the model can be racist, sexist, or just wrong since it is trained on data indiscriminately scraped from the internet.

Pre-training optimizes token level quality where the model is trained to predict the next token accurately. But users don't care about token level quality, they care about the quality of the entire response. Post-training optimizes for generating responses that users prefer.

So for the first issue, it is handled by giving the model a series of training data with prompts and responses which contain the range of requests you want your model to handle such as question answering, summarization, translation, etc.

For the latter, reinforcement learning is used where a reward model scores the foundation model's outputs. The foundation model will then be optimized to generate responses for which the reward model will give maximal scores.

## Sampling: How AI Generates Responses

A model produces its output through a process known as **sampling**. Sampling is what makes AI's output probabilistic, which makes it powerful and creative, but can sometimes cause it to be inconsistent and hallucinate.

Neural networks receive an input and produce an output by computing the possibilities of different outcomes. If it is a classification model on email spams, there are only two possible outcomes which are true or false. For a language model, the model has to generate the next token. If the input is: "What's your favorite color?" There are many possible outputs like green (50%), red (30%), the (0.2%), a (0.1%).

You might think that a language model would just pick the highest probability (like green) but that results in boring outputs. Imagine if for whatever question you ask, you always get back the most common words.

So instead of always picking the most likely token, the model picks red 30% of the time, green 50% of the time, etc.

So the right sampling strategy can make a model generate responses more suitable for your application. One might be to generate more creative responses, while others might be to generate more predictable responses.

![image](https://github.com/user-attachments/assets/f1d0915c-d03c-433f-ac30-4ab482515708)


### Temperature Control

**Temperature** is an example of a way to control that sampling. The problem with probability is that the model can be less creative. For instance, colors red, green, and purple have highest probabilities so the answers you get will always be rigid like "my favorite color is green". Because "the" has a low probability, the model has a low chance of generating a creative sentence such as "My favorite color is the color of a still lake on a spring morning". A higher temperature reduces the probabilities of common tokens, so as a result, increases the probabilities of rarer tokens enabling models to have more creative responses.

**Top-k and Top-p** is another sampling strategy.

## Inconsistency and Hallucinations

The way AI sample their responses make them probabilistic. This nature causes inconsistency and hallucinations. **Inconsistency** is when a model generates very different responses for the same prompt, and **hallucinations** is when a model gives a response that isn't grounded in facts (it makes things up).

If there are essays in the training data saying US presidents are aliens, the model might probabilistically output that the current US president is an alien, and from our perspective, we think the model is making things up. Remember, language models are trained on large amounts of data, and anything with a non-zero probability, no matter how far-fetched or wrong, can be generated by AI.

Inconsistency is caused by randomness in the sampling process, but hallucinations is more nuanced. Because yes, a model samples outputs from all probable options and the output can be something ridiculous if it was in the training set. But how does a model make something up that it never seen before in the training data?

The first hypothesis was expressed by DeepMind, and that is that language models can't differentiate between the data it's given and the data it generates. If you showed the model a picture of a bottle with ingredients, it predicts that the bottle is milk (even though it is not), then in the consecutive computation, it will use that and will invent ingredients related to a bottle of milk. Usually this is mitigated by reinforcement learning where the model is learned to differentiate between user provided prompts and tokens generated by the model.
