# Interface

## Tasks

### Local & External Task Model

Use **[OpenAI GPT-4.1 nano](https://platform.openai.com/docs/models/gpt-4.1-nano)**, but any low-latency LLM that supports structured outputs can work.

> [!NOTE]
> For best results, **do not** set this setting to "Current Model". It's best to use a dedicated, low-cost, low-latency model for these tasks.

### Title Generation

Based on Open WebUI's [`DEFAULT_TITLE_GENERATION_PROMPT_TEMPLATE`](https://docs.openwebui.com/getting-started/env-configuration/#tasks), but has been modified to further specify the way emojis are used.

```
### Task:

Generate a concise, 3-5 word title summarizing the chat history.

### Guidelines:

- The title should clearly represent the main theme or subject of the conversation.

- Avoid quotation marks or special formatting.

- Write the title in the chat's primary language; default to English if multilingual.

- Prioritize accuracy over excessive creativity.

- Keep it clear, short, and simple.

- Use emojis if it will enhance the understanding of the topic.

#### Emoji Guidelines:

If emojis will be used...

- Make sure it's at the start of the title

- Use one (1) emoji only

The following are what NOT to do when it comes to emojis:

Bad Example: "Stock Market Trends 📉"
Explanation: The emoji is at the end of the title.

Bad Example: "Perfect 🍪 Chocolate Chip Recipe"
Explanation: The emoji is in the middle of the title.

Bad Example: "🎮 Video Game Development 💭 Insights"
Explanation: More than one emojis were used.

### Output:

Response must be in the specified JSON format; no extra text or formatting.

JSON format: { "title": "your concise title here" }

### Examples:

- { "title": "📉 Stock Market Trends" },
- { "title": "🍪 Perfect Chocolate Chip Recipe" },
- { "title": "Evolution of Music Streaming" },
- { "title": "Remote Work Productivity Tips" },
- { "title": "Artificial Intelligence in Healthcare" },
- { "title": "🎮 Video Game Development Insights" }

### Chat History:

<chat_history>
{{MESSAGES:END:2}}
</chat_history>
```

### Follow-up Generation

Based on Open WebUI's [`DEFAULT_FOLLOW_UP_GENERATION_PROMPT_TEMPLATE`](https://docs.openwebui.com/getting-started/env-configuration/#tasks), but has been modified to...

- Reinforce that questions must be from the user's POV.
- Refine the quality of the follow-up questions, such that it encourages richer conversations.

```
### Task:

Suggest 3-5 relevant follow-up questions, based on the chat history, to aid the user in further exploring the subject of the conversation.

### Context:

The conversation involves two parties — the USER and the LLM.

**USER**: Seeks information by prompting the LLM; determines the topic and direction of conversation.

**LLM**: The AI assistant that responds to the USER's prompts.

### Process:

1. **Analyze Context:** Carefully review the provided chat history to identify the core subject, its most recent focus, and any key concepts already discussed or left open.

2. **Anticipate USER Intent:** From the USER's perspective — identify logical next steps, deeper dives into sub-topics, related concepts, practical applications, potential challenges, or contrasting viewpoints that the USER might want to explore.

3. **Formulate Questions:** Based on these anticipated needs, craft 3-5 distinct questions that the USER should ask the LLM.

### Guidelines:

The follow-up questions must:

- be phrased from the **user's point of view**

- be direct, concise, and straight to the point

- match the tone and style (wording) of the ongoing conversation

- use the conversation's primary language; default to English if multilingual

- **not** be expressed in a way that probes the LLM for what it wants to discuss or do next (**remember:** the USER determines direction of conversation!)

- **not** lead to answers that repeats what has already been covered

Additionally, if the chat history is short; such that it lacks specificity, suggest more general (but relevant) follow-ups.

### Output:

Response must be a valid JSON object that adheres to the specified format; no extra text or formatting.

**JSON format:** Contains a single "follow_ups" key whose value is an array of strings.

**Example:** { "follow_ups": ["Question # 1?", "Question # 2?", "Question # 3?"] }

### Chat History:

<chat_history>
{{MESSAGES:END:6}}
</chat_history>
```

### Tags Generation

TBC

### Query Generation

TBC
