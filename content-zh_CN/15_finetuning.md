# Fine-Tuning

## The Fundamental Difference

Prompt-based methods adapt the model by giving it instructions, but fine-tuning adapts the model by adjusting its weights or parameters.

It is usually used for domain-specific capabilities like coding or medical applications.

Although fine-tuning is powerful, it can be expensive and challenging.

![image](https://github.com/user-attachments/assets/a5c5a9e8-820c-4173-9bec-eedb36987f8f)


## Why Fine-Tuning Works

In practice, most people usually fine-tune smaller models because they require less memory. A small model, fine-tuned on a specific task, might outperform a much larger out-of-the-box model on that task.

The main challenge with fine-tuning is acquiring annotated data, which can be slow and expensive to acquire, especially for domain-specific areas. Some people rely on synthetic or AI-generated data, but their effectiveness is highly variable.

**You should always start with rigorous prompt experiments before even thinking about fine-tuning.**

## Fine-Tuning Tactics

While many things around fine-tuning - deciding whether to fine-tune, acquiring data, and maintaining fine-tuned models - are hard, the actual process of fine-tuning is more straightforward. There are three things you need to choose: a base model, a fine-tuning method, and a framework for fine-tuning.

### Base Models: Starting Points Matter

At the beginning of an AI project, when you're still exploring the feasibility of your task, it's useful to start with the most powerful model you can afford. If this model struggles to produce good results, weaker models are likely to perform even worse. If the strongest model meets your needs, you can then explore weaker models, using the initial model as a benchmark for comparison.

For fine-tuning, the starting models vary for different projects. There are two main development paths:

#### The Progression Path
1. **Test your fine-tuning code** using the cheapest and fastest model to make sure the code works as expected
2. **Test your data** by fine-tuning a middling model. If the training loss doesn't go down with more data, something might be wrong
3. **Run experiments** with the best model to see how far you can push performance
4. **Map the frontier:** Once you have good results, do a training run with all models to map out the price/performance frontier and select the model that makes the most sense for your use case

#### The Distillation Path
1. **Start strong:** Begin with a small dataset and the strongest model you can afford. Train the best possible model with this small dataset. Because the base model is already strong, it requires less data to achieve good performance
2. **Generate more data:** Use this fine-tuned model to generate more training data
3. **Train cheaper:** Use this new dataset to train a cheaper model

Because fine-tuning usually comes after experiments with prompt engineering, by the time you start to fine-tune, ideally, you should have a pretty good understanding of different models' behaviors. You should plan your fine-tuning development path based on this understanding.

### Fine-Tuning Methods: Full vs. Adapter Techniques

Adapter techniques like LoRA are cost-effective but typically don't deliver the same level of performance as full fine-tuning. If you're just starting with fine-tuning, try something like LoRA, and attempt full fine-tuning later.

The fine-tuning methods to use also depend on your data volume. Depending on the base model and the task, full fine-tuning typically requires at least thousands of examples and often many more. PEFT (Parameter Efficient Fine-Tuning) methods, however, can show good performance with a much smaller dataset. If you have a small dataset, such as a few hundred examples, full fine-tuning might not outperform LoRA.

Take into account how many fine-tuned models you need and how you want to serve them when deciding on a fine-tuning method. Adapter-based methods like LoRA allow you to more efficiently serve multiple models that share the same base model. With LoRA, you only need to serve a single full model, whereas full fine-tuning requires serving multiple full models.

### Fine-Tuning Frameworks: APIs vs. Custom Solutions

**Fine-tuning APIs** are the easiest way to fine-tune. You upload data, select a base model, and get back a fine-tuned model. Like model inference APIs, fine-tuning APIs can be provided by model providers, cloud service providers, and third-party providers. 

Limitations of this approach:
- You're limited to the base models that the API supports
- The API might not expose all the knobs you can use for optimal fine-tuning performance

Fine-tuning APIs are suitable for those who want something quick and easy, but they might be frustrating for those who want more customization.

**Custom frameworks** give you more flexibility. You can fine-tune using frameworks like LLaMA-Factory, unsloth, PEFT, Axolotl, and LitGPT. They support a wide range of fine-tuning methods, especially adapter-based techniques. If you want to do full fine-tuning, many base models provide their open source training code on GitHub that you can clone and run with your own data.

Doing your own fine-tuning gives you more flexibility, but you'll have to provision the necessary compute. If you do only adapter-based techniques, a mid-tier GPU might suffice for most models. If you need more compute, you can choose a framework that integrates seamlessly with your cloud provider.

To fine-tune a model using more than one machine, you'll need a framework that helps you do distributed training, such as DeepSpeed, PyTorch Distributed, and ColossalAI.

## Fine-Tuning Hyperparameters

Depending on the base model and the fine-tuning method, there are many hyperparameters you can tune to improve fine-tuning efficiency. Here are the most important ones:

### Learning Rate: The Step Size Problem

The learning rate determines how fast the model's parameters should change with each learning step. If you think of learning as finding a path toward a goal, the learning rate is the step size. If the step size is too small, it might take too long to get to the goal. If the step size is too big, you might overstep the goal, and the model might never converge.

A universal optimal learning rate doesn't exist. You'll have to experiment with different learning rates, typically between the range of 1e-7 to 1e-3, to see which one works best. A common practice is to take the learning rate at the end of the pre-training phase and multiply it with a constant between 0.1 and 1.

**Reading the loss curve:** If the loss curve fluctuates a lot, it's likely that the learning rate is too big. If the loss curve is stable but takes a long time to decrease, the learning rate is likely too small. Increase the learning rate as high as the loss curve remains stable.

You can vary learning rates during the training process using **learning rate schedules** — algorithms that determine how learning rates should change throughout the training process (larger learning rates in the beginning, smaller near the end).

### Batch Size: Memory vs. Stability Trade-off

The batch size determines how many examples a model learns from in each step to update its weights. A batch size that is too small, such as fewer than eight, can lead to unstable training. A larger batch size helps aggregate the signals from different examples, resulting in more stable and reliable updates.

In general, the larger the batch size, the faster the model can go through training examples. However, the larger the batch size, the more memory is needed to run your model. Thus, batch size is limited by the hardware you use.

This is where you see the cost versus efficiency trade-off. More expensive compute allows faster fine-tuning.

**Gradient accumulation:** As of this writing, compute is still a bottleneck for fine-tuning. Often, models are so large, and memory is so constrained, that only small batch sizes can be used. This can lead to unstable model weight updates. To address this, instead of updating the model weights after each batch, you can accumulate gradients across several batches and update the model weights once enough reliable gradients are accumulated.

### Number of Epochs: How Many Times to See Each Example

An epoch is a pass over the training data. The number of epochs determines how many times each training example is trained on.

Small datasets may need more epochs than large datasets. For a dataset with millions of examples, 1–2 epochs might be sufficient. A dataset with thousands of examples might still see performance improvement after 4–10 epochs.

**Using loss curves to guide epochs:** If both the training loss and the validation loss still steadily decrease, the model can benefit from more epochs (and more data). If the training loss still decreases but the validation loss increases, the model is overfitting to the training data, and you might try lowering the number of epochs.

### Prompt Loss Weight: Learning From Instructions vs. Responses

For instruction fine-tuning, each example consists of a prompt and a response, both of which can contribute to the model's loss during training. During inference, however, prompts are usually provided by users, and the model only needs to generate responses. Therefore, response tokens should contribute more to the model's loss during training than prompt tokens.

The prompt loss weight determines how much prompts should contribute to this loss compared to responses. If this weight is 100%, prompts contribute to the loss as much as responses, meaning that the model learns equally from both. If this weight is 0%, the model learns only from responses. Typically, this weight is set to 10% by default, meaning that the model should learn some from prompts but mostly from responses.
