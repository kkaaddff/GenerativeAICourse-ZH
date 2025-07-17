# Introduction to AI

## What is AI?

Sometimes, when we hear the word too many times, we don't question what it actually means. What is AI? To explain that, let's go back in history.

In the past, in order for computers to do something — show an Instagram post, allow us to buy a product on Amazon, calculate our taxes — we had to program the computer by giving it explicit instructions. So we as humans programmed the application step by step to solve these problems. What that means is, we solved the problem first as humans, and then gave these instructions to the computer.

With AI, instead of us solving the problem first, we teach the machine to solve the problem by giving it a bunch of different examples or data.

A very basic example I always give: say we have an app that predicts house prices based on size. We can write that logic ourselves, say the house price is always the size times 3. But what if instead of us writing the formula, we showed the computer millions of examples or data points of house prices and sizes? The computer figures out from all these data points, "Oh, actually it's not just the size times 3, maybe it's size times 1.5." Even if you showed me as a human millions of examples of house prices and size, I will come up with a better formula than just guessing that the house price is equal to size times 3, but of course humans can't look at millions of examples.

So AI is teaching the computer to learn from large datasets or examples. AI is excellent in cases where we want to look at data and solve problems. Let's take Instagram feed recommendation: without AI, we can let the human solve the problem (i.e., decide what to show you), or we can let AI see your browsing history, and based on that data, decide what to show you.

## AI Model

I want to define what an AI model is. Because so far, we kept it at a high level — we said that AI is teaching computers to solve problems from large datasets.

What happens behind the scenes? Purely math. And don't be scared of math, because a mathematical function is nothing more than something that takes an input and produces an output. There are functions that double the input, so if you input 2, you get 4 (y = 2x). Some functions reverse the input, so if you put in 2, you get -2 (y = -x). These are all mathematical functions.

If we go back to our example of a function that predicts house price based on its size: instead of us writing this mathematical function, we show the computer millions of examples of house prices based on their size and we let it come up with the mathematical formula itself.

**That mathematical formula is the AI model.** So there is an AI model to predict house prices — that is a mathematical function to predict house prices. The input is house size, and output is house price. There is an AI model that puts the relevant content on your Instagram feed based on your browsing activities — that is another mathematical function trained specifically on Instagram browsing data. The input is browsing activities, the output is the Instagram post. But again, these are functions that computers came up with based on millions of data examples.

More precisely in practice, what happens is that humans choose the base mathematical function. So for example, we know that the house price is proportional to the size (the bigger the house, the more expensive it is), so we can model it using a linear function like (y = ax + b). Then the computer looks at all of the examples of input and output pairs (house prices and their size) then determines what the most appropriate values for a and b are (these are called **parameters**). Previously, we just guessed that the house price is 3 times its size (y = 3x + 0), but with AI, we let it look at multiple examples and come up with accurate values for a and b.

## Types of AI

There are 3 primary methods of teaching the computer from a large subset of data:

**Supervised Learning, Unsupervised Learning, and Reinforcement Learning.**

### 1. Supervised Learning

Supervised learning is the first and most common approach to train AI models. We teach the machine to solve problems by giving it a set of labeled data — not just any data, **labeled data**. Let's use the house size predictor as an example. To teach the computer, we give it a series of input/output pairs of house size and house price, all of that data is labeled. It says, "For this house size, this is the price," and we have millions of such data. The computer keeps trying different formulas, compares it with the set of labeled data until it arrives at the perfect formula. This is what we call training the AI or ML model — this is why it is expensive to train AI models because the computer keeps trying different formulas for millions of datasets. But now we have an AI model that predicts house prices based on size.

If we want to build an AI model, a mathematical function that can predict what picture is a dog or cat, we give it millions of labeled pictures of dogs and cats. It's all labeled, hence why it is called **supervised**. We as humans did the hard work of labeling the data, and hence are supervising the machine to learn from that data.

![image](https://github.com/user-attachments/assets/4c1b4c17-b1f6-4070-b528-689c6b681668)



### 2. Unsupervised Learning

Unsupervised learning is when the data only has inputs with no outputs. We are not trying to predict something like whether an email is spam; instead, we are simply discovering patterns in the data. Shopping data is the easiest example to think of. The computer can discover patterns from this massive data and conclude that customers who buy bread often buy milk. This data is not labeled or doesn't have outputs — it is simply buying behavior consisting of information like "this customer bought eggs, this bought oats, this bought milk," and from all that data, it discovers patterns.

So supervised learning is: "Here's house size and price, learn to predict prices based on size." Unsupervised is: "Here's shopping data, find interesting patterns."

On Amazon, product recommendations ("customers who bought x also bought y") is an example of an unsupervised ML model. The next time you're on Netflix, you are bombarded with unsupervised ML models telling you "people who watched this series also watched this."

![image](https://github.com/user-attachments/assets/c5538fab-6499-4179-be97-b74a1761fd0e)


### 3. Reinforcement Learning

Instead of learning from labeled data or finding patterns in unlabeled data, reinforcement learning is about learning through trial and error. It's literally like training a dog. When a dog does something good, you give it a treat. Over time, the dog learns which actions lead to treats and which don't.

In reinforcement learning, the ML model takes actions, and based on these actions, it receives rewards or penalties. As an example, an AI chess player uses reinforcement learning. It played millions of matches and received positive rewards for winning moves and negative rewards for losing moves. Over time, the machine discovers strategies that human players never considered. Self-driving vehicles is another example. A self-driving car gets rewards for staying in its lane, maintaining safe distances, and reaching its destination.

When we talk about rewards, of course we're not giving the computer a treat. The reward is simply a numerical value that tells the algorithm whether an action was good or bad. So positive numbers indicate good actions, negative numbers indicate bad actions.

With reinforcement learning, we're not giving the AI a dataset to study upfront. Instead, we're placing it in an environment (real or simulated) where it takes actions, results are observed, and it either gets a reward or penalty.

With AI models like ChatGPT, the model is trained using a supervised model where it is trained on millions of labeled data like books or websites. The model then uses reinforcement learning where it generates multiple possible responses to a given prompt, and these responses are rated based on how helpful or accurate they are either by humans or by another AI system. The model receives these ratings as rewards.

![image](https://github.com/user-attachments/assets/d72c893d-1390-4583-8007-50aa0c6bf6d1)


## Why AI Became Mainstream

Did you know that AI has been around since the 1950s? The question is, why did it suddenly become mainstream? We've had Google Translate for a long time, which is AI. We had fraud detection in credit cards, which is AI. We had Instagram feed algorithms for a long time. Why did it suddenly kick off?

Sure, products like ChatGPT became viral, but here's the actual reason: In order for AI to work, you need 3 things:

1. **Algorithms**
2. **Computers** 
3. **Data**

What companies like OpenAI did is they created a general-purpose model, exposed them to the public that anyone can use. And these models understood natural language really well, and it turns out that models that can understand natural language are general-purpose models, meaning they can be used to solve many everyday use cases like writing code, customer support, etc.

In the past, you had to train an army of machine learning engineers and invest in massive amounts of infrastructure to train your own AI model.

So what happened now is, every company can now become an AI company.
