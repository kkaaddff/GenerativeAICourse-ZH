# AI Agents

## Overview: From Manual to Autonomous

Most software today requires humans to click through interfaces, or make decisions, or manually execute workflows.

Think of customer service teams manually reviewing refund requests.

We tried to solve this problem with traditional automation tools, but they only work on perfectly predictable workflows, as it relies on a series of if and else statements.

A lot of business processes though, are complex, unpredictable, and require dynamic decision making.

Many people define AI agents as LLMs that can take action. Historically, LLMs were about generating a response, agents are about doing.

**But from first principles, an AI agent is simply an application that uses an LLM as its decision making engine to execute actions independently.** The key word is independently — it doesn't rely on you to take actions (telling it what to do).

A refund approval AI agent is an application that can take actions (just like all applications) like approving a refund, forwarding it to a human, executing the refund, etc. The only difference is that it is not the human that makes these decisions, it is the LLM.

### Traditional Automation vs. AI Agents

You can build a refund approval process using traditional automation with simple logic such as:
```
If amount > $500 AND no manager approval THEN reject
```

With an AI agent, it can review customer's purchase history, considers the reason for return, evaluates the company policy in context, then makes the decision. The agent can recognize workflow completion (refund is processed and customer is notified) and can self-correct (I sent the wrong refund amount, let me issue a correction).

![image](https://github.com/user-attachments/assets/04699a4f-256b-4a74-9986-60e2b550094e)


## Why AI Agents Suddenly Exploded

1. **Decision making capability:** The rise of reasoning models made AI agents capable of making complex decisions.
2. **Communication protocols:** The rise of communication protocols like MCP (more on that in the next module) made the process of building complex agents a lot simpler.

### Microsoft's Agent Definition and Vision

The way we define agents at Microsoft, and what we use as our frame of reference to build products and capabilities for customers, is **a system that humans can delegate tasks to, and these tasks will only become more complex over time**.

**Reasoning:** Because of the rise of reasoning models, agents are now able to do increasingly complex tasks, especially software engineering.

### The Agentic Web

**Runtime Components:**
- **Reasoning layer:** The core decision-making capability that determines what actions to take
- **Memory:** This is fundamental software engineering. We've explored solutions like RAG and long context windows, and there's ongoing development like Talk agents. The goal is to remember interactions and solve problems the same way it did last time — just like what you would expect from a human you delegate tasks to
- **Tools:** The actual capabilities that allow agents to take actions in the world

**Protocols:** 
- **MCP (Model Context Protocol):** You need agents to take actions, which means they have to communicate with other systems. MCP is like HTTP for AI agents — it provides a standardized way for agents to interact with different services and tools
- **A2A (Agent-to-Agent):** Communication protocols that enable agents to work together

## When to Build AI Agents

### 1. Complex Decision Making
When workflows require judgement calls that can't be captured in rules. For instance, a traditional system might automatically approve refunds under $50, but an agent can evaluate a $45 refund request for a product the customer has returned 3 times before.

### 2. Unmaintainable Rule System
If your rule system has grown to hundreds of conditional statements.

### 3. Unstructured Data Workflows
If you are processing documents, emails, or natural language. For instance, insurance claim agents can read accident reports, medical records, etc. Something impossible with traditional parsing.

### Economic Calculation
Agents of course cost more per operation than traditional automation, but they eliminate the human labor required for complex decisions. If you're paying people $50/hour to review cases that an agent can handle for $2 per case, and you process 1000 cases monthly, you save $48,000 per month minus $2000 in agent costs.

## Components of Agents

### 1. Model (The Brain)
This is the brain. Different models have different capabilities and different costs. Always build your prototype with the most capable model to establish a performance baseline, set up evaluation metrics (accuracy and task completion rates), and replace with smaller models where acceptable results are maintained.

Usually, larger models are used for complex decision points. As an example, use an SLM for simple data retrieval (get customer order history) but an LLM for refund decisions (should we approve this $500 refund for a customer who's returned 5 items this month).

### 2. Tools
They could be data tools like database queries, they could be communication tools like sending emails or data modification (updating a CRM record), or could be orchestration tools like calling another agent. Each tool needs standardized definitions in your code with clear parameters and return values. Poor tool documentation causes agents to misuse APIs.

**Note:** You might have heard terms like function calling. The difference between function calling and tools, is that tools are the actual implementation (like the python functions that do the work) while function calling is the LLM's ability to tell your app "I need access to the weather function". You need both in an agent, but Semantic Kernel abstracts away function calling so they can appear to be the same thing.

**Example:**
```python
@function_tool
def process_refund(customer_id: str, order_id: str, amount: float, reason: str):
    """
    Process a refund for a customer order.
    
    Args:
        customer_id: Unique customer identifier
        order_id: Order to refund
        amount: Refund amount in dollars (must be <= original order total)
        reason: Business reason for refund (required for accounting)
    
    Returns:
        {"success": bool, "refund_id": str, "message": str}
    """
    # Implementation here
    return {"success": True, "refund_id": "REF123", "message": "Refund processed"}
```

### 3. Instructions
This defines how agent behaves, what it's allowed to do, and how it should handle edge cases. Instead of writing new procedures, adapt your current policies. If your customer service team follows a 12-step refund process, that becomes your agent's instruction set.

Capture edge cases as well like: What if customer provides partial information? What if the API call fails? What if the customer asks something outside your domain?

## Orchestrator

Once you have an agent with a model, tools, and instructions, you need to orchestrate these workflows from start to finish. A single LLM can't handle multi-step processes that require checking conditions, calling multiple tools, and adapting based on results.

### Single Agent Systems
Easier to build, and typically relies on prompt templates, where instead of creating separate agents for different use cases, you use a single flexible prompt that accepts variables.

### Multiple Agent Systems
Used when complex logic is required and when the number of tools become large. There are two main patterns:

1. **Hub and Spoke:** A central manager agent coordinates specialized worker agents
2. **Decentralized Pattern (Peer to Peer):** There is no central coordinator

The former is used when you need to synthesize the results from multiple agents or when you need one agent to maintain conversation context with a user.

## Handling Memory

One of the biggest challenges in building production AI agents is managing memory. We already explored how we solve the problem of conversation memory, such as summarizing past conversations, or even using RAG for more long-term storage. Remember, LLMs are stateless — they don't remember anything between API calls as every request is independent.

The other type of memory is **learning memory**. Ideally, we want the agent to learn from all past interactions so it becomes better. An example of how this is implemented is using RAG, so we store successful resolution examples and retrieve them for similar cases.

## Agentic RAG: Beyond Traditional Retrieval

We already explored a traditional RAG system. The user sends the prompt, we check the vector store for data related to the prompt, that is combined with the prompt and sent to the LLM. LLM in this case literally just summarizes the answer, nothing more.

But LLMs have much more capabilities than just summarizing the prompt & the relevant chunks retrieved from the vector store. **Agentic RAG** is a more sophisticated RAG implementation, where when the user asks a question, the agent analyzes the question, then decides the retrieval strategy (maybe I need to bring data from vector database 2 and 3), then reasons on top of the retrieved information, then retrieves more, until they get to the final answer. So it is a more sophisticated form of RAG.

## Guardrails

AI agents can take actions that affect real customers, they can access sensitive data, and they can represent your brand. Unlike chatbots that only generate text, agents need protection against dangerous output actions.

You need a balance. Overly restrictive guardrails can block legitimate user requests. But the business reality is that a single leaked customer database or an unauthorized $10k refund can cost more than months of guardrail development.

### Types of Guardrails

1. **Relevance Classifier:** This keeps agents working within their intended scope. So it prevents customer service agents from answering random questions instead of solving customer problems. SLMs are typically used to classify the input relevance.

2. **Safety Classifier:** Detects prompt injection and jailbreak attempts. Typically, fine-tuned models trained on prompt injection patterns are used.

3. **PII Filter:** Prevents accidental exposure of PII information.

4. **Moderation Filter:** Blocks harmful content including hate speech or harassment. OpenAI has moderation APIs that can be used.

5. **Human in the Loop:** For low or medium risk tasks (like sending emails, updating customer records) that might not need human intervention. Human approval can be used for high risk like processing refunds or cancelling orders.

6. **Rule Based Protection:** This is to quickly block known threats. Like SQL injection filters, input length limit, character encoding filters (prevent unicode based attacks).

7. **Output Validation:** Ensure agent responses align with brand guidelines and business policies. Things like tone analysis, policy compliance, legal compliance, etc.

### Strategy for Building Guardrails

**Phase 1:** Focus on data privacy and content safety. These are highest impact risks that can cause immediate business damage (PII exposure, prompt injection, harmful content that damages brand image).

**Phase 2:** Add guardrails based on real-world failures. Monitor your deployed agent and implement guardrails for actual problems encountered like user attempts to get free products through social engineering.

**Phase 3:** Optimize for security and UX. Reduce false positives that block legitimate requests, customize guardrail sensitivity based on user context (VIP customers get different treatment).

### Human Intervention
Guardrails are great, but agents at the moment still need human in the loop. Even for fully autonomous agents, you need humans to be available just in case, and this is triggered when you exceed failure thresholds or for high-risk actions.

## Building Agents from scratch vs using a "managed" Agent such as Azure AI Agent

You can build AI agents using Semantic Kernel from scratch, where you manage things like tool access (you write the code yourself) or memory. Or you can use Azure AI Agent which is a managed service. It is infrastructure that runs on Azure. Microsoft manages the servers, persistent storage (things like file storage, conversation threads, and vector databases are managed server-side), built-in tools like code interpreter all running on Microsoft's secure environment, and enterprise integration to AI search, functions, etc.

You can certainly use Semantic Kernel with standard LLM APIs but you need to manage conversation state management, tool execution & security, scaling, vector databases, etc. Azure AI Agent can handle all that for you but you may be concerned about higher costs or vendor lock-in. Of course, Semantic Kernel can be used to connect to Azure AI Agent service. 
