# 📖 Chapter 1 Notes: Building AI Applications with LangChain

This chapter introduces the fundamental building blocks required to create AI applications using **LangChain**, **OpenRouter**, and **Pydantic**.

---

# 🏗️ Overall Workflow

Every LangChain application generally follows this flow:

```text
Load API Key
      │
      ▼
Create LLM
      │
      ▼
Collect User Input
      │
      ▼
Create Prompt
      │
      ▼
Generate Response
      │
      ▼
(Optional) Convert to Structured Output
      │
      ▼
Display Results
```

---

# 🌱 1. Environment Variables (`.env`)

## 🎯 Purpose

Store sensitive information like API keys securely instead of hardcoding them inside the program.

### You'll Learn

- Loading a `.env` file
- Reading environment variables
- Checking whether an API key exists

### Libraries

- `python-dotenv`
- `os`

> 💡 **Best Practice:** Never hardcode API keys inside your Python files.

---

# 🤖 2. ChatOpenAI (OpenRouter)

## 🎯 Purpose

Create an LLM that communicates with OpenRouter.

### You'll Learn

- Creating a `ChatOpenAI` object
- Connecting to OpenRouter
- Choosing a model
- Setting the temperature

### Important Parameters

| Parameter | Purpose |
|-----------|---------|
| `base_url` | OpenRouter API endpoint |
| `model` | Specifies which LLM to use |
| `temperature` | Controls creativity (0 = deterministic, 1 = more creative) |
| `api_key` | Authentication |

---

# 🙋 3. User Input

## 🎯 Purpose

Collect information dynamically from the user.

Example inputs:

- 🌍 Destination
- 📅 Number of Days
- 🎒 Travel Style
- 💰 Budget Level

Each input should be stored in a separate variable.

---

# 💬 4. ChatPromptTemplate

## 🎯 Purpose

Create **dynamic prompts** using placeholders.

Instead of manually writing:

```text
Create a 3-day trip to Tokyo.
```

Use placeholders like:

- `{destination}`
- `{days}`
- `{travel_style}`
- `{budget}`

LangChain automatically replaces them with user input.

---

# 🎭 Understanding Prompt Messages

`ChatPromptTemplate` structures prompts using different message types.

| Message Type | Purpose |
|--------------|---------|
| 🛡️ **System** | Defines behavior, role, tone, and rules |
| 🙋 **User** | Represents the user's request |
| 🤖 **Assistant** | Provides example responses (optional) |

---

## 🛡️ System Message

The **System Message** defines **how the AI should behave** throughout the conversation.

Examples:

- 👤 Persona
- 🎨 Tone
- 📚 Expertise
- 📏 Constraints

Example instructions:

- You are Peter Parker.
- Answer professionally.
- Explain like a teacher.
- Keep responses under 100 words.

> ⭐ The System Message has the **highest priority** among all prompt messages.

---

## 🙋 User Message

The **User Message** contains the user's request.

Example:

```
Tell me about {memory}
```

If the user enters:

```
Aunt May
```

LangChain automatically creates:

```
Tell me about Aunt May
```

---

## 🤖 Assistant Message

The **Assistant Message** represents a previous AI response.

It is commonly used in **Few-Shot Prompting**, where example conversations guide the model toward the desired response style.

Example:

```
System:
You are a helpful assistant.

User:
What is AI?

Assistant:
AI stands for Artificial Intelligence.

User:
Can you explain it simply?
```

---

# 🎭 Persona vs 🎨 Tone

These terms are often confused but serve different purposes.

## 👤 Persona = Who the AI is

Examples:

- Peter Parker
- Sherlock Holmes
- Senior Python Developer
- Travel Expert

Persona changes the **identity** of the AI.

---

## 🎨 Tone = How the AI speaks

Examples:

- 😊 Friendly
- 💼 Professional
- 😂 Humorous
- 📖 Formal

Tone changes the **communication style**, not the identity.

---

## 📚 Expertise

Defines the AI's domain knowledge.

Example:

- Python Expert
- Doctor
- Travel Planner
- Financial Advisor

---

## 📏 Constraints

Control the format of the response.

Examples:

- Under 100 words
- Bullet points only
- Respond in Spanish
- Explain simply

---

# 🎯 Combining Instructions

A System Message can combine multiple instructions together.

Example:

| Feature | Example |
|---------|---------|
| 👤 Persona | Travel Expert |
| 🎨 Tone | Friendly |
| 📚 Expertise | Budget Travel |
| 📏 Constraint | Under 150 words |

Combining instructions results in more consistent and controlled responses.

---

# 📨 Manual Messages

Besides `ChatPromptTemplate`, LangChain also allows manual conversations using message objects.

You'll work with:

- 🛡️ `SystemMessage`
- 🙋 `HumanMessage`
- 🤖 `AIMessage`

These simulate a real chat history.

Example flow:

```
System:
You are a travel expert.

Human:
I love adventure trips.

AI:
Adventure trips are exciting!

Human:
Suggest one travel tip.
```

---

# 🏗️ 5. Pydantic

## 🎯 Purpose

Define the structure that the AI must follow.

Instead of generating plain text,

the model returns a structured Python object.

Example fields:

- Trip Title
- Destination
- Introduction
- Places to Visit
- Food Recommendation
- Travel Tip
- Estimated Budget

---

# 🏷️ 6. Field()

Each attribute inside a Pydantic model should include a meaningful description.

Descriptions help the LLM understand:

- What information belongs in each field
- The expected content
- The purpose of the attribute

This often improves the quality of structured responses.

---

# 📦 7. Structured Output

Normally an LLM returns plain text.

Example:

```
Welcome to Tokyo...
```

With **Structured Output**, the LLM follows a predefined schema and returns a structured object instead.

Example:

```
TravelPlan
├── Trip Title
├── Destination
├── Introduction
├── Places to Visit
├── Food Recommendation
├── Travel Tip
└── Estimated Budget
```

This makes the output:

- ✅ Predictable
- ✅ Easy to validate
- ✅ Easy to use in Python

---

# 🎯 8. Accessing Individual Fields

Instead of printing the whole object, access each field separately.

Examples:

- Trip Title
- Destination
- Introduction
- Food Recommendation
- Travel Tip
- Estimated Budget

For lists (like places to visit), print each item individually.

---

# 🚀 Complete Application Flow

```text
Load API Key (.env)
        │
        ▼
Create ChatOpenAI Model
        │
        ▼
Collect User Input
        │
        ▼
Create ChatPromptTemplate
        │
        ▼
Generate Messages
        │
        ▼
Convert LLM → Structured Output
        │
        ▼
Generate Pydantic Object
        │
        ▼
Access Individual Fields
        │
        ▼
Display Final Result
```

---

# 📝 Chapter Summary

By the end of this chapter, you should understand:

- 🌱 Environment Variables (`.env`)
- 🤖 Creating an LLM using `ChatOpenAI`
- 🙋 Collecting user input
- 💬 Creating prompts with `ChatPromptTemplate`
- 🛡️ Using **System**, **User**, and **Assistant** messages
- 🎭 Persona vs Tone
- 📨 Manual Messages (`SystemMessage`, `HumanMessage`, `AIMessage`)
- 🏗️ Creating schemas using **Pydantic**
- 🏷️ Using `Field()` descriptions
- 📦 Generating **Structured Output**
- 🎯 Accessing individual fields from a Pydantic object

> 💡 **Key Takeaway:** Modern AI applications don't just generate text—they combine **prompt engineering**, **message-based conversations**, and **structured outputs** to build reliable, production-ready applications.