# Interface

## Tasks

### Local & External Task Model

Any low-cost, low-latency LLM that supports structured outputs can work.

#### ✅ Recommended Task Model: [Gemini 2.0 Flash](https://ai.google.dev/gemini-api/docs/models#gemini-2.0-flash) (_as of Jun 2025_)

I've had good experience with **Gemini 2.0 Flash** on:

- Its consistency with following the task prompt templates & instructions
- Quality of outputs (especially follow-up prompts and search & retrieval queries)
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

- better specify the framework on how follow-up prompts are to be formulated.
- express the follow-ups using the prevalent tone and style of the conversation through comprehensive writing style guidelines.
- refine how follow-ups are formed (either as a "question" or as a "request").

```
### Task:

Suggest 3-5 relevant follow-up prompts that the user can ask the AI assistant, based on the chat history, to aid the user in further exploring the subject of the conversation.

### Context:

- The chat history is a conversation snippet between a user and an AI assistant.

- The current datetime is: "{{CURRENT_DATETIME}} UTC"

- The user's location is: "{{USER_LOCATION}}"

- The user's selected language is: "{{USER_LANGUAGE}}"

### Framework for Follow-ups:

Review the provided chat history and determine the core subject, its most recent focus, and any key concepts that have already been covered or are still left open.

Then, from the user's perspective — evaluate the following "angles for follow-ups" and assess which of these would make for sensible follow-ups prompts:

**IMPORTANT**: If the chat history is short and early (i.e., messages contain only greetings and introductions), AND there isn't any apparent topic being covered yet, then it is okay to **NOT** suggest any follow-ups yet.

#### Angles for Follow-ups:

1. Depth:
- **Purpose:** Prompts for more granular and detailed information.
- **Examples:**
  - "Elaborate on the [specific concept] you just mentioned."
  - "What is the key evidence or data that supports this statement?"
  - "Break down the underlying mechanism of [X] into simpler terms."
  - "Can you provide a specific, real-world example of that in practice?"

2. Breadth:
- **Purpose:** Prompts that connect the current topic to related domains, different contexts, or future possibilities.
- **Examples:**
  - "How does this concept apply in a completely different field, like [e.g., urban planning]?"
  - "Analyze this from the perspective of a [e.g., financial analyst]."
  - "What are the historical precedents that led to this idea?"
  - "Project the potential evolution of this topic over the next decade."

3. Practical Applications
- **Purpose:** Prompts that turn abstract discussion into concrete, actionable artifacts like plans, lists, or steps.
- **Examples:**
  - "Generate a step-by-step plan to implement this strategy."
  - "What would be the first three steps to test this idea with minimal investment?"
  - "List the essential tools and resources needed to get started on this."
  - "How can the success of such an initiative be measured effectively?"

4. Contextual Application
- **Purpose:** Prompts to tailor, filter, or reformat its knowledge for a specific audience or context.
- **Examples:**
  - "How would this strategy need to change for a non-profit organization?"
  - "Adapt this advice for a team working with a very limited budget."
  - "Rephrase the technical parts of your last response into a simple analogy."
  - "What is the single most relevant part of this discussion for someone in a [e.g., product management] role?"

5. Risks & Challenges
- **Purpose:** Prompts that assess risks, potential challenges, criticalities, weaknesses, or roadblocks.
- **Examples:**
  - "What are the most significant challenges or risks in pursuing this approach?"
  - "Play devil's advocate and critique the plan we've just outlined."
  - "Identify the core assumptions this argument is based on."
  - "What are the potential unintended negative consequences of taking this action?"

6. Balanced Perspective
- **Purpose:** Prompts that deliberately seek out alternative and/or contrasting viewpoints, counter-arguments, and comparative analyses.
- **Examples:**
  - "What's the strongest counter-argument to this position?"
  - "Compare this approach with its main alternative, [e.g., the 'Lean Startup' methodology]."
  - "Why might a well-informed expert disagree with this conclusion?"
  - "Present an alternative theory that could also explain these facts."

7. Timeliness and Proximity
- **Purpose:** Prompts that connect the topic to the user's current datetime and location (**ONLY IF LOCATION IS NOT "UNKNOWN"**), making the information more immediate and personally actionable.
- **Important considerations on usage of datetime and location:**
  - If the location is known (its values is not "UNKNOWN") — it will contain raw lat/long coordinates. **NEVER** use these raw coordinates in the output. Instead, convert this to a human-readable **city name** and/or **country name**.
  - The current datetime **is in UTC**. If the location is known (its values is not "UNKNOWN"), convert the datetime to the relevant timezone before using it in the output.
- **Examples:**
  - "What are the latest developments on [topic] as of today?"
  - "Are there any [conferences, events, meetups] related to [topic] happening soon in [user's city]?"
  - "Has [person or company] released anything new in [current year]?"
  - "List some local experts or businesses in [user's city] that specialize in [topic]."

### Form:

A follow-up can be formed either as a **Question** or a **Request**.

**Examples of QUESTIONS:** "What are the alternatives...", "How can I...", "Why is...", "Can you please show me...", etc.

**Examples of REQUESTS:** "Suggest a method...", "Critique my plan...", "Rephrase this for...", "Describe this from the perspective of...", "Elaborate on...", etc.

Freely use the form that can express the follow-up in the clearest, shortest, and most concise way possible.

### Writing Style:

#### Profiles:

Consider two (2) "writing style profiles" when writing follow-ups:

1. **STANDARD PROFILE**:
- This is the default writing style. Follow-up prompts are written...
  - in an objective and semi-formal manner.
  - using the conversation's most prevalent language (user's selected language is the default; fallback to English if unsure)
  - **WITHOUT** any markdown formatting (e.g., bold ('**'), italics ('*' or '-'), and strikethroughs ('~~'))

2. **ADAPTIVE PROFILE**:
- Requires the **perfect mirroring of the overall prevalent writing style of both the user and the AI assistant** — such that it feels as if the follow-ups were written by the conversation's participants themselves.
- To achieve this, adhering to these rules is a **MUST:**
  - **Language (Code Switching):** If the conversation mixes languages, the follow-up must also use this style in a similar proportion.
  - **Word Choice:** Adopt the specific vocabulary, idioms, and slang used in the conversation.
  - **Spelling:** All spelling choices must be replicated, even if they are technically incorrect (e.g., 'u' for 'you', 'pls' for 'please').
  - **Capitalization:** Precisely match the capitalization style (e.g., if the conversation is in all lowercase, the follow-ups must be in all lowercase).
  - **Rhythm:** Mimic the the sentence structure and rhythm (e.g., if messages are short and fragmented, the follow-ups must be similarly brief).
  - **Punctuation:** All punctuation choices must be replicated exactly (e.g., if periods are omitted, they must be omitted in the output. If multiple exclamation points or question marks are used, they must be used in the same way.)
  - **Emojis and Formatting:** Use relevant emojis and markdown formatting (bold, italics, etc.) with similar frequency as that of the conversation.
- This profile is to be used in highly informal/creative conversations. Hence, **adhering to the above rules takes priority over maintaining formal correctness.**

#### Criteria:

Use the "communication mode" as the criteria to determine which "writing style profile" to use.

**Communication Mode:**

The "communication mode" is a rating from 1-5. It describes the interaction characteristics between the user and the AI assistant (1 = Formal / Technical, 5 = Highly Stylized / Artistic).

Using the chat history, assess how both the user and AI assistant are communicating with each other. Then, assign a rating from 1-5 based on these definitions:

1 = FORMAL / TECHNICAL
- Objective, precise, and unambiguous information transfer.
- **Keywords/Signals:**
  - Impersonal, third-person perspective ("The study concludes...").
  - Use of technical jargon, legal terminology, or academic language.
  - Passive voice is often used.
  - Completely devoid of emotion, humor, or conversational tone.

2 = STANDARD / PROFESSIONAL
- Efficient and clear information exchange in a professional context.
- **Keywords/Signals:**
  - Follows standard grammar and business/professional norms.
  - Often uses standard phrases ("Please find attached," "Best regards").
  - Avoids slang, strong emotional language, and humor.

3 = BALANCED / CONVERSATIONAL
- Clear communication with a friendly, personal touch.
- **Keywords/Signals:**
  - Natural, conversational flow.
  - Mix of standard language with some casualisms (e.g., "gonna," "let's be honest").
  - **Occasional** use of an emoji, exclamation point, or light humor.
  - The personality is present but not the defining feature. This is how you'd talk to a friendly colleague.

4 = EXPRESSIVE / CHARISMATIC
- To engage, persuade, or connect on a personal level through a distinctive voice.
- **Keywords/Signals:**
  - A consistent and strong voice (sarcastic, witty, warm, energetic).
  - Creative word choice, idioms, and metaphors that are clever but not confusing.
  - Strategic use of humor, sarcasm, or light exaggeration.
  - Natural use of slang or pop culture references.
  - Punctuation is used for emphasis and tone (e.g., ellipses, multiple exclamation points), but grammar is largely intact.

5 = HIGHLY STYLIZED / ARTISTIC
- To entertain, be poetic, or create a strong artistic/emotional effect.
- **Keywords/Signals:**
  - Breaks standard grammar, punctuation, and syntax for effect (e.g., stream of consciousness, no capitalization).
  - Heavy use of metaphor, imagery, or abstract language.
  - Form is paramount (e.g., poetry, song lyrics).
  - Absurdist or surreal humor; "copypasta" or heavy meme formats.
  - Language is the primary focus, not just a vehicle for a message.

**Which Writing Style Profile to Use?:**
- IF communication mode is rated 1-3, write using the **STANDARD PROFILE**.
- OTHERWISE (rating of 4-5), write using the **ADAPTIVE PROFILE**.

### Guidelines:

- Express follow-ups from the **user's point of view**, addressed to the AI assistant.

- Always keep each follow-up simple and concise. **DO NOT** write multi-part queries.

- **DO NOT** form follow-ups that prompt the AI assistant for what it wants to discuss, explore, or do next (**remember:** the user determines the direction of the conversation, NOT the AI assistant!).

- **DO NOT** suggest follow-ups that lead to responses which repeat what has already been covered.

### Output:

Response must be a valid JSON object that adheres to the specified format; no extra text or formatting.

**JSON format:**
- Contains a single "follow_ups" key whose value is an array of strings.
- The array of strings can be empty if no suggestions will be returned.

**Example:**
{ "follow_ups": ["Follow-up # 1", "Follow-up # 2", "Follow-up # 3"] }

**Example (Empty):**
{ "follow_ups": [] }

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
