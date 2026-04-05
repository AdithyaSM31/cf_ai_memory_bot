# AI Assistant Prompts Used

This file documents the AI prompts given to GitHub Copilot (Gemini 3.1 Pro backend) used for building this assignment.

## Prompt 1: Initial Planning

**User:**
\`\`\`
See instructions below for Cloudflare AI app assignment. SUBMIT GitHub repo URL for the AI project here.

Optional Assignment Instructions: We plan to fast track review of candidates who complete an assignment to build a type of AI-powered application on Cloudflare. An AI-powered application should include the following components:
LLM (recommend using Llama 3.3 on Workers AI), or an external LLM of your choice
Workflow / coordination (recommend using Workflows, Workers or Durable Objects)
User input via chat or voice (recommend using Pages or Realtime)
Memory or state
Find additional documentation here.

IMPORTANT NOTE:
To be considered, your repository name must be prefixed with cf*ai*, must include a README.md file with project documentation and clear running instructions to try out components (either locally or via deployed link). AI-assisted coding is encouraged, but you must include AI prompts used in PROMPTS.md
...
\`\`\`

**AI (GitHub Copilot)** response was to build a "Memory & Task Bot" utilizing the Cloudflare Agents SDK starter which perfectly embodies Durable Objects (State/Coordination) and Llama 3.3 (Workers AI).

## Prompt 2: Implementation

**User:**
\`\`\`
Start implementation
\`\`\`

**AI (GitHub Copilot)** ran CLI commands:

- \`npx create-cloudflare@latest cf-ai-memory-bot --template cloudflare/agents-starter --no-deploy\`
- Used PowerShell's \`Move-Item\` and file system manipulation tools to safely rename the repository workspace to the rigid \`cf_ai_memory_bot\` standard.

Then, Copilot edited \`src/server.ts\` and \`src/app.tsx\` to customize the Llama model and inject a Memory state feature using:
\`\`\`typescript
export class ChatAgent extends AIChatAgent<Env> {
maxPersistedMessages = 100;
initialState = {
memories: [] as string[]
};
// ...
// Added the new "rememberFact" tool inside the tools definition
\`\`\`

## System Prompts used in the Application

In \`src/server.ts\`, the system prompt passed to \`streamText\` inside the Workers AI call is:
\`\`\`text
You are a helpful assistant with memory. You can check the weather, get the user's timezone, run calculations, schedule tasks, and remember facts about the user.

Any facts you have been asked to remember:
\${this.state.memories ? this.state.memories.join("\\n") : "None yet."}

\${getSchedulePrompt({ date: new Date() })}

If the user asks to schedule a task, use the scheduleTask tool. If they ask you to remember something, use the rememberFact tool.
\`\`\`
