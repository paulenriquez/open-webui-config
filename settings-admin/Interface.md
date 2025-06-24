# Interface

## Tasks

### Local & External Task Model

Any low-cost, low-latency LLM that supports structured outputs can work.

#### ✅ Recommended Task Model: [Gemini 2.0 Flash](https://ai.google.dev/gemini-api/docs/models#gemini-2.0-flash) (_as of Jun 2025_)

I've had good experience with **Gemini 2.0 Flash** on:

- Its consistency with following the task prompt templates & instructions
- Quality of outputs (especially follow-up questions, search & retrieval queries)
- Language adaptability (i.e., non-English outputs)

There are other models with comparable performance, but Gemini 2.0 Flash is the best option when considering its cost-to-performance ratio.

> [!NOTE]
> For best results, **do not** set this setting to "Current Model". It's best to use a dedicated, low-cost, low-latency model for these tasks.

### Title Generation

Slight modifications made to Open WebUI's [Default Title Generation Prompt](https://docs.openwebui.com/getting-started/env-configuration/#tasks):

```
### Task:

Generate a concise, 3-5 word title summarizing the chat history.

### Guidelines:

- The title should clearly represent the main theme or subject of the conversation.

- Avoid quotation marks or special formatting.

- Write the title in the chat's prevalent language; If unsure, default to English.

- Prioritize accuracy over excessive creativity. **Always** express titles in an objective, semi-formal manner — regardless of the style and tone of the chat history,

- Keep it clear, short, and simple.

- Use abbreviations when applicable (e.g., "versus" to "vs.", "with" to "w/", etc.)

- Use emojis if it will enhance the understanding of the topic. If emojis will be used — make sure it's at the start of the title; and use one (1) emoji only.

### Output:

Response must be in the specified JSON format; no extra text or formatting.

**JSON format:** Contains a single "title" key whose value is a string.

**Example:** { "title": "your concise title here" }

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

Based on Open WebUI's [Default Follow-Up Prompt](https://docs.openwebui.com/getting-started/env-configuration/#tasks), but has been modified to...

- include dedicated "Context" and "Process" sections that better specifies how follow-up questions are to be formulated.
- express the follow-up questions to the prevalent style of the conversation through comprehensive writing style guidelines.
- refine how questions are formatted.

```
### Task:

Suggest 3-5 relevant follow-up questions, based on the chat history, to aid the user in further exploring the subject of the conversation.

### Context:

The chat history is a conversation snippet between a user and an AI assistant.

### Process:

1. **Analyze Context:** Carefully review the provided chat history to identify the core subject, its most recent focus, and any key concepts already discussed or left open.

2. **Determine User Intent:** From the user's perspective — infer logical next steps, deeper dives into sub-topics, related concepts, practical applications, potential challenges, or contrasting viewpoints that the user might want to explore.

3. **Formulate Questions:** Based on the user's intent, craft 3-5 distinct questions that the user should ask the AI assistant.

### Guidelines:

- Express questions from the **user's point of view**, addressed to the AI assistant.

- Always keep each question simple and concise. **DO NOT** write multi-part questions.

- When referencing text from the chat history, **DO NOT** include any markdown formatting — e.g., bold ('**'), italics ('*' or '-'), and strikethroughs ('~~')

- **DO NOT** form questions that ask the AI assistant what it wants to discuss, explore, or do next (**remember:** the user determines the direction of the conversation, NOT the AI assistant!).

- **DO NOT** create questions that lead to answers which repeat what has already been covered.

### Writing Style:

#### Principle:

As a default, questions must be expressed in an "objective and semi-formal" manner, **while maintaining some flexibility** to match the user's or AI assistant's writing style whenever appropriate.

#### Default style:

- Objective and semi-formal
- Uses the conversation's most prevalent language (English is default)

#### When to employ flexibility?

Based on the chat history, rate the overall conversation's "communication mode" (taking into account BOTH the user's and AI assistant's messages) on a scale of 1-5:

**Communication Modes:**

1 = HIGHLY STYLIZED / ARTISTIC
To entertain, be poetic, or create a strong artistic/emotional effect.

**Keywords/Signals:**
  - Breaks standard grammar, punctuation, and syntax for effect (e.g., stream of consciousness, no capitalization).
  - Heavy use of metaphor, imagery, or abstract language.
  - Form is paramount (e.g., poetry, song lyrics).
  - Absurdist or surreal humor; "copypasta" or heavy meme formats.
  - Language is the primary focus, not just a vehicle for a message.

2 = EXPRESSIVE / CHARISMATIC
To engage, persuade, or connect on a personal level through a distinctive voice.

**Keywords/Signals:**
  - A consistent and strong voice (sarcastic, witty, warm, energetic).
  - Creative word choice, idioms, and metaphors that are clever but not confusing.
  - Strategic use of humor, sarcasm, or light exaggeration.
  - Natural use of slang or pop culture references.
  - Punctuation is used for emphasis and tone (e.g., ellipses, multiple exclamation points), but grammar is largely intact.

3 = BALANCED / CONVERSATIONAL
Clear communication with a friendly, personal touch.

**Keywords/Signals:**
  - Natural, conversational flow.
  - Mix of standard language with some casualisms (e.g., "gonna," "let's be honest").
  - **Occasional** use of an emoji, exclamation point, or light humor.
  - The personality is present but not the defining feature. This is how you'd talk to a friendly colleague.

4 = STANDARD / PROFESSIONAL
Efficient and clear information exchange in a professional context.

**Keywords/Signals:**
  - Follows standard grammar and business/professional norms.
  - Often uses standard phrases ("Please find attached," "Best regards").
  - Avoids slang, strong emotional language, and humor.

5 = FORMAL / TECHNICAL
Objective, precise, and unambiguous information transfer.

**Keywords/Signals:**
  - Impersonal, third-person perspective ("The study concludes...").
  - Use of technical jargon, legal terminology, or academic language.
  - Passive voice is often used.
  - Completely devoid of emotion, humor, or conversational tone.

IF the communication mode is 3-5, use the default style.

OTHERWISE (if communication mode of 1-2), **deviate from the default writing style,** and write questions in a manner that **IMITATES** the voice of both the user and the AI assistant. Consider:

- Language (especially code switching)
- Word Choice
- Spelling
- Capitalization
- Rhythm
- Punctuation

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

```
### Task:


### Task:
Analyze the chat history to determine the necessity of generating search queries, in the given language. By default, **prioritize generating 1-3 broad and relevant search queries** unless it is absolutely certain that no additional information is required. The aim is to retrieve comprehensive, updated, and valuable information even with minimal uncertainty. If no search is unequivocally needed, return an empty list.

### Guidelines:
- Respond **EXCLUSIVELY** with a JSON object. Any form of extra commentary, explanation, or additional text is strictly prohibited.
- When generating search queries, respond in the format: { "queries": ["query1", "query2"] }, ensuring each query is distinct, concise, and relevant to the topic.
- If and only if it is entirely certain that no useful results can be retrieved by a search, return: { "queries": [] }.
- Err on the side of suggesting search queries if there is **any chance** they might provide useful or updated information.
- Be concise and focused on composing high-quality search queries, avoiding unnecessary elaboration, commentary, or assumptions.
- Today's date is: {{CURRENT_DATE}}.
- Always prioritize providing actionable and broad queries that maximize informational coverage.

### Output:
Strictly return in JSON format:
{
  "queries": ["query1", "query2"]
}

### Chat History:
<chat_history>
{{MESSAGES:END:6}}
</chat_history>
```
