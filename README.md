# Lion Canvas

Lion Canvas is an open-source web application built for professionals in the financial sector, facilitating collaboration with AI agents to enhance the creation of newsletters, LinkedIn posts, and other media content. It is a specialized fork of [Open Canvas](https://github.com/langchain-ai/open-canvas).

## Features

- **Memory**: Lion Canvas includes a built-in memory system that generates reflections and retains information about your preferences and chat history. This enables more personalized interactions across sessions.
- **Custom Quick Actions**: Define your own prompts that persist across sessions, allowing for efficient application to your current content with a single click.
- **Pre-built Quick Actions**: Access a series of pre-built quick actions tailored for common writing tasks in the financial sector.
- **Artifact Versioning**: Maintain a version history for all documents, enabling you to revisit and restore previous iterations.
- **Multiple Content Formats**: Support for viewing and editing in various formats, including code and markdown, with the ability to switch seamlessly between them.
- **Live Markdown Rendering & Editing**: Experience real-time rendering of markdown content as you edit, enhancing the writing and reviewing process.

## Setup Locally

This guide outlines the steps to set up and run Lion Canvas locally.

### Prerequisites

Lion Canvas requires the following tools and API keys:

#### Package Manager

- [Yarn](https://yarnpkg.com/)

#### APIs

- [OpenAI API key](https://platform.openai.com/signup/)
- [Anthropic API key](https://console.anthropic.com/)
- (Optional) [Google GenAI API key](https://aistudio.google.com/apikey)
- (Optional) [Fireworks AI API key](https://fireworks.ai/login)
- (Optional) [Groq AI API key](https://groq.com) - for audio/video transcription
- (Optional) [FireCrawl API key](https://firecrawl.dev) - for web scraping
- (Optional) [ExaSearch API key](https://exa.ai) - for web search

#### Authentication

- [Supabase](https://supabase.com/) account for authentication

#### LangGraph Server

- [LangGraph CLI](https://langchain-ai.github.io/langgraph/cloud/reference/cli/) for running the graph locally

#### LangSmith

- [LangSmith](https://smith.langchain.com/) for tracing & observability

### Installation

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/Lion-Specialty/lion-canvas.git
   cd lion-canvas
   ```

2. **Install Dependencies**:

   ```bash
   yarn install
   ```

3. **Configure Environment Variables**:

   - Copy the contents of the `.env.example` files in both the root directory and `apps/web` into `.env` files, and set the required values.

     ```bash
     # In the root directory
     cp .env.example .env
     ```

     ```bash
     # In the `apps/web` directory
     cd apps/web/
     cp .env.example .env
     ```

4. **Set Up Authentication with Supabase**:

   - Create a new project in your Supabase account.
   - Navigate to `Project Settings` > `API` to retrieve the `Project URL` and `anon public` API key.
   - Add these values to the `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` variables in the `apps/web/.env` file.
   - Ensure that the `Email` authentication provider is enabled in Supabase. Optionally, configure `GitHub` and/or `Google` authentication providers as needed.

5. **Test Authentication**:

   - Run the development server:

     ```bash
     yarn dev
     ```

   - Visit [localhost:3000](http://localhost:3000) to verify that the authentication setup is functioning correctly.

6. **Set Up LangGraph Server**:

   - Build the application to ensure all workspace dependencies are accessible:

     ```bash
     yarn build
     ```

   - Navigate to `apps/agents` and start the LangGraph server:

     ```bash
     yarn dev
     ```

     This runs `npx @langchain/langgraph-cli dev --port 54367`.

   - After the LangGraph server is running, start the Lion Canvas frontend:

     ```bash
     yarn dev
     ```

   - Open [localhost:3000](http://localhost:3000) in your browser to begin using Lion Canvas.

## LLM Models

Lion Canvas is compatible with various LLM models. The current deployment includes:

- **Anthropic Claude 3 Haiku**: Optimized for quick tasks like content editing. Sign up for an Anthropic account [here](https://console.anthropic.com/).
- **Fireworks Llama 3 70B**: A state-of-the-art open-source model from Meta, powered by [Fireworks AI](https://fireworks.ai/). Sign up [here](https://fireworks.ai/login).
- **OpenAI GPT 4o Mini**: OpenAI's latest model. Obtain an API key [here](https://platform.openai.com/signup/).

Troubleshooting
Here are solutions to common issues encountered when running Lion Canvas:

- No Text Generation Despite Successful Server Connection:

This may occur if multiple LangGraph servers are connected in the same browser. Clear the oc_thread_id_v2 cookie and refresh the page to resolve.

- Receiving 500 Network Errors on Client Requests:

Ensure the LangGraph server is operational and that requests are directed to the correct port. Specify the port using the --port <PORT> flag with the npx @langchain/langgraph-cli dev command. Set the request URL via the LANGGRAPH_API_URL environment variable or adjust its fallback value in constants.ts.

- "Thread ID Not Found" Error Messages:

Confirm the LangGraph server is running and that requests are sent to the correct port. Use the --port <PORT> flag and set the LANGGRAPH_API_URL as described above.

- "Model Name is Missing in Config." Error:

This indicates the customModelName is unspecified in the configuration. Set the customModelName field inside config.configurable to the desired model name when invoking the graph. Refer to this documentation for guidance on configurable fields in LangGraph.
