# Interface

## Tasks

### Local & External Task Model

Use **[OpenAI GPT-4.1 nano](https://platform.openai.com/docs/models/gpt-4.1-nano)**, but any low-latency LLM that supports structured outputs can work.

> [!NOTE]
> For best results, **do not** set this setting to "Current Model". It's best to use a dedicated, low-cost, low-latency model for these tasks.

### Title Generation

The prompt is based on Open WebUI's [`DEFAULT_TITLE_GENERATION_PROMPT_TEMPLATE`](https://docs.openwebui.com/getting-started/env-configuration/#tasks), but has been modified to further specify the way emojis are used.

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

TBC

### Tags Generation

TBC

### Query Generation
