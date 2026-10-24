### The 4 Non-Negotiable Rules

- **Zero-Paste for Solutions:** Never paste an entire project requirement (e.g., "Build the TOP Etch-a-Sketch") into a prompt. If you don't do the decomposition into smaller steps yourself, you bypass the core lesson.
    
- **The 20-Minute Struggle Window:** When you hit a bug, spend at least 15–20 minutes reading the browser console, using `console.log`, and tracing variable state yourself before asking for help. AI is the third resort, after your brain and the documentation.
    
- **Hints Over Patches:** Ask _why_ an error occurred, never _how to fix it_. Make the AI point to the line or concept that failed rather than printing corrected code.
    
- **The Explain-Back Test:** If an AI helps you understand an elusive concept, close the chat and write a 2-sentence summary or a mini code example in your own notes. If you can't explain it simply, you haven't learned it.
    

### 4 High-Leverage Prompt Patterns

**1. The Socratic Debugger** When your code produces an unexpected result or throws an error, don't ask for the correction. Force the AI to guide your thinking:

> _"Here is a function I wrote and the error message from my browser console: [paste snippet + error]. Act as a strict coding mentor. Do NOT provide corrected code. Instead, ask me two questions about my assumptions or point out which line violates expectations so I can track it down myself."_

**2. The Conceptual Drill-Down** When TOP's documentation leaves you foggy on abstract browser/language mechanics (like event bubbling, closures, or the call stack):

> _"Explain how event delegation works under the hood using a real-world restaurant analogy. After explaining, give me a 3-question conceptual quiz to test if I actually grasped it."_

**3. The Edge-Case Inspector (Code Review)** After you have completely finished a project and all your tests pass, use AI as a code reviewer to find blind spots:

> _"Here is my working Calculator logic: [paste code]. It currently works for standard inputs. Review it purely for unhandled edge cases (like decimal spamming or floating-point precision) and code readability. Point out the weaknesses conceptually without rewriting my functions."_

**4. Reverse Rubber-Ducking**Instead of asking how something works, explain what you _think_ is happening and let the AI catch flaws in your mental model:

> _"I'm going to explain in plain English how asynchronous JavaScript and the event loop handle a fetch call: [type your explanation]. Tell me what parts of my mental model are accurate and where I am technically mistaken."_

### The System Prompt to Keep AI Honest

If you use a tool that allows custom instructions or system prompts, add this single directive:

Plaintext

```
You are a senior software engineering mentor. Your primary goal is to help me develop strong problem-solving and debugging intuition. Never write full functions or solve coding problems for me. Provide hints, explain technical concepts using clear analogies, suggest edge cases, and ask guiding questions. If I ask for code, refuse and guide me through pseudocode or mental models instead.
```