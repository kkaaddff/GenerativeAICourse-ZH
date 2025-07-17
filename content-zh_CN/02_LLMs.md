# How LLMs Work

We're going to get more technical as we move on, but still within the spirit of sticking to first principles.

## The Core Mechanism: Next Word Prediction

We all used tools like ChatGPT — how do these actually work behind the scenes? A large language model takes a series of text input like "The cat sat on" and literally predicts what the next word is. It predicts that "the" is 70% likely, "a" is 20% likely, "my" is 10% likely, so it picks "the". It becomes "the cat sat on the". Then predicts the next word, which is "mat" as most likely. Each new word is based on all previous words, like a sophisticated auto-complete.

![image](https://github.com/user-attachments/assets/9331a11c-f988-4099-bb76-ecc77fd835c4)


So if you ask ChatGPT, "What is the capital of France?", it looks at the series of words "the" "capital" "of" "France" then predicts "is" as logical next word, then "Paris". The model is literally just predicting likely next words based on patterns it learned. It does this well because it is trained on massive amounts of text.

You might say, "OK, I get that when I ask ChatGPT a question, it takes my prompt or question, which is a series of words, and predicts what the next words are. How does it generate text, like write an essay, when I ask it to?"

**Text generation is just repeated word prediction.** If you ask the model: "Write an essay about cats", it looks at the prompt "write an essay about cats" then it predicts the most likely word which is "cats" then "are" then "fascinating" and so on one word at a time. It is literally thinking one word at a time and picking the most appropriate word each time.

## The Mathematical Foundation

With house prices, f(x) = wx + b, we find w and b values that work best (parameters). With language models, each word becomes a list of numbers like [0.2, -0.5, 0.8], and these numbers, for each word, are known as **embeddings**. Each word is represented by a series of numbers capturing meaning, sentiment, etc.

So "cat" becomes [0.2, -0.5, 0.8], "sat" becomes [0.3, 0.6, -0.2]. Cool, each word is represented by a series of numbers capturing different features of the word. The LLM model takes these numbers (embeddings) as input, uses a complex mathematical function (based on neural network) with billions of parameters and the output is a next word prediction.

These parameters or variables in a mathematical function capture so many things like grammar rules, which sequence of words makes sense, how to combine words. This is why when you throw a sentence into the model, the model has billions of parameters that are able to predict what the next word is.

## Training Process: Self-Supervision

The billions of parameters in this function are what gets adjusted during training to make better word predictions. So you show the model a lot of text, you start with random parameter values, model makes prediction, adjusts parameters to improve predictions.

What happens in practice: you literally train the model on millions of sentences or text examples. Starting with: "The cat sat on the mat", we show the model "The cat sat on the", we know the correct answer is "The cat sat on the mat" and we adjust parameters until it predicts the word "mat". If it predicts the word "box", that is wrong, so we keep adjusting parameters. Then we move on to next sentence "Paris the capital", we change parameter values until it comes up with the word "of" as the next prediction.

### Self-Supervision: Why This is Revolutionary

Supervised machine learning (ML algorithms trained using labeled data) is expensive — if it costs 5 cents for one person to label one image, it would cost 50k to label a million images — and often not scalable. Language models use **self-supervision**. Say you train the model with the sentence "The cat sat on the mat", the model sees "The cat sat on the" and predicts "mat", so the original sentence itself provides the correct answer. So no human labeling is required.

So self-supervised learning means language models can learn from text sequences without requiring any labeling. Since text sequences are everywhere (books, blog posts, articles, etc), there is massive amount of training data allowing models to scale up to become LLMs.

Think about how many sentences we have — this is why the training is rigorous. Billions of parameters to adjust, millions of examples to learn from, takes weeks or months to train. Once we find parameters that can capture patterns from all examples, we are confident that the model can predict likely next words.

Once we find values for these parameters, we're good — we can input any word, and we can easily predict the next word.

## Tokens

These words that are transformed to numbers are known as **tokens**. Whenever you use LLMs hosted by other companies, you are paying per tokens.

![image](https://github.com/user-attachments/assets/7a7bd7e9-4134-4480-a22c-a71cc3872277)


## Transformer Architecture

In 2017, Google released a paper called "Attention is All You Need." Transformer architecture was invented in this paper. The inventors themselves did not realize the breakthrough they made. In 2018, GPT-1 was released, then GPT-2 in 2019, GPT-3 in 2020, and then in 2022 ChatGPT was released leveraging GPT-3 in combination with reinforcement learning from human feedback.

OpenAI's first GPT model had 117M parameters, now it is trillions.

## Why Large Language Models Are General Purpose

Language models, thanks to their scale, are capable of a wide range of tasks. Previous models were specific to specific tasks like sentiment analysis or translation (language models can do both). If you're a retailer and want to generate product descriptions, you can use a model that generates accurate descriptions but might fail to capture the brand's voice or messaging. With language models, you can craft detailed instructions with examples of desired product descriptions, you can ground it in specific data or even fine-tune it.
