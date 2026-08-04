# 🎭Adding Tone & Persona to LLM Responses using `ChatPromptTemplate`

In LangChain, `ChatPromptTemplate` is used to structure prompts for chat-based LLMs. It allows you to define different types of messages, such as **system**, **user**, and **assistant**.

## 📝 Example

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI
import os

llm_openai = ChatOpenAI(
    base_url="https://openrouter.ai/api/v1",
    model="openai/gpt-oss-20b:free",
    temperature=0.7,
    api_key=os.getenv("OPENROUTER_API_KEY")
)

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "You are Peter Parker."),
    ("user", "Tell me about {memory}")
])

user_input = input("Who is your favorite person? ")

ready_prompt = prompt_template.invoke({"memory": user_input})

response = llm_openai.invoke(ready_prompt)

print(response.content)
```

---

# Understanding the Prompt Structure

# 🛡️ 1. System Message

The **system message** defines how the LLM should behave throughout the conversation.

```python
("system", "You are Peter Parker.")
```

This tells the model:

- Act like Peter Parker.
- Respond from Peter Parker's perspective.
- Use Peter Parker's personality, knowledge, and speaking style.

The system message has the **highest priority** among prompt messages.

---

# 👤 2. User Message

The **user message** represents the input provided by the user.

```python
("user", "Tell me about {memory}")
```

When the user enters:

```
Aunt May
```

the actual prompt becomes:

```
Tell me about Aunt May
```

---

# Is This Adding Tone?

**Partially.**

The system prompt primarily assigns a **role (persona)**.

That role naturally influences:

- Tone
- Vocabulary
- Style
- Perspective
- Personality

So,

```python
("system", "You are Peter Parker.")
```

does **more than just set the tone**.

It makes the model behave as Peter Parker.

---

# 🎭 Persona vs 🎨 Tone

## 👤 Persona = **Who the AI is**

Example:

```python
("system", "You are Peter Parker.")
```

Possible response:

> Aunt May has always been my biggest inspiration. She taught me that with great power comes great responsibility.

---

## 🎨 Tone = **How the AI speaks**

Example:

```python
("system", "Answer in a friendly and humorous tone.")
```

Possible response:

> That's a fantastic question! 😄 Let me tell you...

---

## Style

Defines the writing style.

Example:

```python
("system", "Answer like Shakespeare.")
```

Possible response:

> Verily, thou seekest knowledge most profound...

---

## 🧠 Expertise

Defines the AI's profession or domain knowledge.

Example:

```python
("system", "You are a senior Python developer.")
```

Possible response:

> The issue occurs because the variable hasn't been initialized before use.

---

## 📏 Constraints

Define specific rules for responses.

Example:

```python
("system", "Keep every answer under 50 words.")
```

or

```python
("system", "Always respond in bullet points.")
```

---

# 🎉 Combining Everything

The best prompts usually combine multiple instructions.

```python
prompt = ChatPromptTemplate.from_messages([
    (
        "system",
        """
        You are Peter Parker.
        Be friendly and witty.
        Keep responses under 100 words.
        Explain things simply.
        """
    ),
    ("user", "Tell me about {memory}")
])
```

Now the AI has:

| Feature | Value |
|---------|-------|
| 👤 Persona | Peter Parker |
| 😊 Tone | Friendly & Witty |
| 📏 Constraint | Under 100 words |
| 📚 Style | Simple explanations |

---

# Why Use `ChatPromptTemplate`?

Instead of writing one long prompt string, `ChatPromptTemplate` separates different message types.

| Message Type | Purpose |
|--------------|---------|
| **System** | Defines behavior, role, tone, and rules |
| **User** | Represents the user's input |
| **Assistant** | Provides example responses (few-shot prompting) |

Example:

```python
ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("user", "What is AI?"),
    ("assistant", "AI stands for Artificial Intelligence."),
    ("user", "Can you explain it simply?")
])
```

Including **assistant** messages is useful for **few-shot prompting**, where you show the model examples of the desired response format.

---
# 📌 Key Takeaways

- 🎭 **System Message** defines the AI's **role, personality, tone, and rules**.
- 🙋 **User Message** contains the user's question.
- 🤖 **Assistant Message** provides example responses (optional).
- 👤 **Persona** = *Who the AI is.*
- 🎨 **Tone** = *How the AI speaks.*
- 📏 System prompts can also control **expertise, language, response length, formatting, and style**.
- 🚀 A well-crafted system prompt leads to more consistent, natural, and engaging AI responses.

> 💡 **Pro Tip:** Think of the **System Message** as the AI's **operating manual**. The better the instructions, the better the responses!
