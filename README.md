# Layered Agent Workflow for VS Code Copilot

This repository contains a structured, multi-agent workflow for software development using custom VS Code Copilot chat modes and reusable prompts. It establishes a clear, layered process that enhances agent performance through deliberate **Context Engineering**.

## Core Philosophy: Context Engineering

The primary goal of this workflow is to manage the LLM's context window effectively. By dividing the development process into distinct phases—**Research**, **Planning**, and **Implementation**—we practice **Context Engineering**. This ensures that each agent receives a tailored, minimal context sufficient only for its specific job. This results in more reliable, efficient, and predictable outcomes from the AI agents.

-----

## The Agents & Their Roles

This system is built around three specialized agents (chat modes):

  * **🧑‍🔬 The Researcher:** The information gatherer. Its job is to explore the codebase, search external documentation, and synthesize all relevant information into a foundational `RESEARCH.md` document.
  * **📝 The Planner:** The strategist. It takes the curated research and, through a collaborative process with the user, breaks it down into a high-level `PLAN.md` and a series of discrete, actionable tasks.
  * **🛠️ The Implementer:** The executor. A "worker bee" that takes a single task file and executes the instructions precisely as written, without deviation.
  * **🧑‍🏫 The Instructor:** The meta-agent. Its purpose is to help you create new behavioral rules for the other agents, formalizing them into `.instructions.md` files.

-----

## Workflow Steps

The process is divided into two main phases, moving from human-led direction to automated agent execution.

### Phase 1: Human-Led Interaction

1.  **Goal Definition (User)**
    *   The process begins when the user provides a high-level development goal.

2.  **Research (🧑‍🔬 Researcher Agent)**
    *   The user and the **Researcher** collaborate to explore the codebase, gather requirements, and analyze dependencies.
    *   **Handoff:** The agent produces a `/tmp/RESEARCH.md` file for user review and approval.

3.  **Planning (📝 Planner Agent)**
    *   The user provides the approved `RESEARCH.md` to the **Planner**.
    *   Through a collaborative dialogue, they create a high-level strategy and break it down into discrete, actionable tasks.
    *   **Handoff:** The agent produces:
        *   A `/tmp/PLAN.md` file outlining the overall strategy.
        *   One or more task files (e.g., `/tmp/TASK_01.md`, `/tmp/TASK_02.md`).

### Phase 2: Agent-Led Execution

4.  **Implementation (🛠️ Implementer Agent)**
    *   The **Implementer** receives a single task file (e.g., `/tmp/TASK_01.md`) and executes the required code changes.

5.  **Verification & Reporting**
    *   The implemented changes are verified.
    *   **On Success:** A **Completion Report** is generated.
    *   **On Failure:** A **Failure Report** is generated, detailing the errors.

6.  **Iteration Loop**
    *   In case of a failure, the **Failure Report** is escalated back to the **Planner**, allowing the user to revise the plan and generate a new task file.

-----

## The Workflow in Action

Here is the intended step-by-step process, from initial idea to final implementation.

### 1\. The Research Phase

Your journey begins with the **🧑‍🔬 Researcher**. Activate it in the chat (`@workspace #research`) and use the `/start-research` command to begin a scripted dialogue where you provide the project requirements. Once the agent has enough information, instruct it to begin its investigation. To conclude this phase, use the `/create-research-document` command, which will guide the agent to produce the final `/tmp/RESEARCH.md` file for your review.

### 2\. The Planning Phase

With the research complete, switch to the **📝 Planner** (`@workspace #plan`). Use the `/create-plan` command and point it to the `RESEARCH.md` file. You will first approve a high-level strategy and then collaboratively define each specific implementation task. This process results in a `/tmp/PLAN.md` and one or more `/tmp/TASK_XX_description.md` files.

### 3\. The Implementation Phase

Now, switch to the **🛠️ Implementer** (`@workspace #implement`). Assign a single task using the `/implement-prompt` command and provide the path to a task file (e.g., `/tmp/TASK_01_description.md`). The agent will confirm its understanding before proceeding. After executing the task, it will use the `/report-status` command to inform you of its success or failure.

-----

## Guiding Agent Behavior with the Instructor

Over time, you may notice patterns of unwanted behavior from the agents. Instead of manually correcting them every time, you can create permanent behavioral rules using the **🧑‍🏫 Instructor** agent.

Activate it in the chat (`@workspace #InstructionMaker`) and use the `/create-rule` command. The Instructor will guide you through a collaborative process to:

1.  **Diagnose** the problematic behavior.
2.  **Define** a clear, unambiguous directive to correct it.
3.  **Formalize** the directive into a `.instructions.md` file.

These instruction files are automatically loaded by the agents, providing persistent, fine-grained control over their execution. This is the primary mechanism for evolving and improving the agent team's reliability.

-----

## Advanced Workflows

  * **Revising a Plan:** If the Implementer reports a failure, take its detailed error report back to the **Planner**. Use the `/revise-plan` command to collaboratively debug the issue and generate a corrected task file. The Implementer can then be given the new instructions using the `/reconcile-plan` command.
  * **Creating a Permanent Rule:** If a problem is likely to recur, use the **Instructor** agent's `/create-rule` command to establish a permanent behavioral guideline that prevents the issue in the future.
  * **Saving Your Work:** At the end of a session with the Researcher or Planner, use the `/summarize-session` command. This creates a permanent, traceable summary of your work linked to issue trackers.
  * **Managing Long Conversations:** If a chat thread grows too long and agent performance degrades, use the `/thread-dump` command to generate a concise handoff briefing. You can then paste this into a new chat session to continue your work with a fresh context.

-----

# Resources

  * [**Creating Chat Modes**](https://code.visualstudio.com/docs/copilot/customization/custom-chat-modes)
  * [**Creating Prompt Files**](https://code.visualstudio.com/docs/copilot/customization/prompt-files)
