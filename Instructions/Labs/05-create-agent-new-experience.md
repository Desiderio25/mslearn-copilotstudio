---
lab:
  title: Create an agent using the new Copilot Studio experience
  module: Create agents in Microsoft Copilot Studio
  description: In this lab, you will use the new Copilot Studio experience to create an instruction-driven agent, add a prebuilt action, and test autonomous reasoning behavior.
  duration: 30 minutes
  level: 200
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Create an agent using the new Copilot Studio experience

## Scenario

In this exercise, you will:

- Switch to the new Copilot Studio experience
- Create an agent using natural language instructions
- Add a prebuilt action the agent can use autonomously
- Test the agent and observe its reasoning behavior
- Refine instructions based on test results

This exercise will take approximately **30** minutes to complete.

## What you will learn

- How the new Copilot Studio experience differs from the classic interface
- How to configure an agent using instructions instead of topic trees
- How the agent reasons autonomously to select knowledge and actions
- How to iterate on instructions to improve agent behavior

## High-level lab steps

- Switch to the new Copilot Studio experience
- Create an agent with a description
- Write and refine agent instructions
- Add a prebuilt action
- Test and iterate

## Prerequisites

- Have a Microsoft Entra ID account
- Have a Copilot Studio license or have signed up for a [free trial](https://go.microsoft.com/fwlink/p/?linkid=2252605).
- Have access to a Power Platform environment and a solution where you can create agents and related assets.
- You can use:
  - the environment and **Lab Exercises** solution created in the **ILT Setup** lab, or
  - your own existing environment and solution.
- If you do not already have an environment and solution prepared, complete the steps in the **ILT Setup** lab before continuing.

> [!IMPORTANT]
> The new Copilot Studio experience is the redesigned interface that replaces the classic topic authoring canvas. Steps and screenshots in this lab reflect the new experience. If your environment shows the classic interface, follow the prompt to switch to the new experience before proceeding.

## Key concept: Instruction-driven agents

The new Copilot Studio experience replaces the topic authoring canvas with a simpler, instruction-driven model:

| Classic experience | New experience |
|---|---|
| Agent behavior defined by topic trees and trigger phrases | Agent behavior defined by natural language instructions |
| Author builds explicit conversation flows node by node | Agent reasons dynamically about how to respond |
| Topics, conditions, and branches control conversation routing | Instructions, knowledge, and actions determine what the agent does |
| Actions require building a Power Automate workflow, configuring inputs/outputs, and adding it as a tool | Prebuilt connector actions are added directly in Copilot Studio — no workflow authoring required |

The new experience is best suited for agents that need to handle open-ended, context-dependent conversations. The classic experience (covered in later labs) remains the right choice when you need a guaranteed, auditable sequence of steps — for example, collecting structured data in a specific order.

## Exercise 1 - Switch to the new Copilot Studio experience

### Task 1.1 – Open the new experience

1. Navigate to the **Copilot Studio** home page at `https://copilotstudio.microsoft.com/` and sign in if prompted.

1. At the top of the page, verify that you are working in the environment you want to use for this exercise.

1. If you see a banner or notification prompting you to **Try the new experience**, select it.

   Alternatively, select your profile icon in the upper-right corner and look for an option to switch to the **New experience** or **Preview**.

   > [!NOTE]
   > The new experience may be the default in your environment. If you do not see a switch option, you are already using the new experience and can proceed to Exercise 2.

### Task 1.2 – Review the new interface

Take a moment to review how the new experience is organized before creating your agent.

1. In the left-hand navigation, note the main sections: **Agents**, **Tools**, and **Knowledge**.

1. The **Agents** page shows any existing agents. Agents created in the new experience do not have a topic canvas — their behavior is defined entirely by instructions, knowledge, and actions on the agent's **Overview** page.

1. Notice there is no **Topics** section in the navigation. In the new experience, topics are replaced by actions and instructions.

## Exercise 2 - Create an agent

In this exercise, you will create an IT support agent for a fictional company called Contoso. The agent will help employees troubleshoot common IT issues and submit support tickets.

### Task 2.1 – Create the agent

1. Select **Agents** in the left-hand navigation.

1. Select **+ New agent**.

1. In the agent creation dialog, enter the following description:

   ```prompt
   You are an IT support agent for Contoso. You help employees troubleshoot common IT issues such as password resets, software installation problems, and network connectivity. When you cannot resolve an issue, you help the employee submit a support ticket.
   ```

1. Select **Create** or **Next** to provision the agent.

   > [!NOTE]
   > The new experience uses your description to automatically generate a set of initial instructions. Review them before continuing.

1. In the **Solution** settings, verify that the **Lab Exercises** solution is selected and the schema name prefix is **fab**. Update these if needed.

### Task 2.2 – Review and refine the auto-generated instructions

1. On the agent's **Overview** page, locate the **Instructions** section.

1. Review the instructions that were generated from your description. They should describe the agent's purpose, tone, and general behavior.

1. Select **Edit** in the **Instructions** section.

1. Update the instructions to include the following guidelines, either by typing or by using **Edit with Copilot**:

   ```prompt
   ## Guidelines
   - Always respond in a professional and friendly tone.
   - For password reset requests, direct the employee to the self-service portal at https://aka.ms/sspr before offering to raise a ticket.
   - For issues you cannot resolve, collect the employee's name, email address, and a brief description of the issue before submitting a ticket.
   - Do not speculate about hardware failures. Always recommend contacting the IT desk directly for physical hardware issues.
   ```

1. Select **Save**.

   > [!NOTE]
   > Instructions in the new experience are the primary way to control agent behavior. Well-written instructions reduce the need for additional configuration and make the agent more predictable.

## Exercise 3 - Add an action

In Lab 03, adding an action required you to build a Power Automate workflow from scratch, configure its inputs and outputs, publish it, and then add it to the agent as a tool. In the new experience, you can add a prebuilt connector action directly in Copilot Studio without leaving the page or authoring a flow. This exercise demonstrates that difference.

### Task 3.1 – Add a prebuilt action

1. On the agent's **Overview** page, select the **Actions** tab or locate the **Actions** section.

1. Select **+ Add action**.

1. Browse the available prebuilt actions and select **Send an email** from the **Microsoft 365** or **Outlook** connector.

   > [!NOTE]
   > If **Send an email** is not available in your environment, select any available prebuilt action and adapt the remaining steps accordingly.

1. Sign in when prompted to authorize the connector.

1. Select **Add and configure**.

1. In the **Details** section, update the **Description** to:

   `Send a support ticket notification email to the IT helpdesk when an issue cannot be resolved by the agent.`

1. In the **When this action may be used** setting, select **Only when referenced in instructions or by the agent**.

1. Select **Save**.

### Task 3.2 – Reference the action in instructions

The agent will only use the action if its instructions tell it when to do so.

1. Return to the **Instructions** section on the **Overview** tab and select **Edit**.

1. Add the following line under your existing guidelines:

   ```prompt
   - When an employee's issue cannot be resolved, use the Send an email action to notify the IT helpdesk at helpdesk@contoso.com with the employee's name, email, and issue description.
   ```

1. Select **Save**.

## Exercise 4 - Test and refine

In this exercise, you will test the agent and observe how it reasons before responding.

### Task 4.1 – Open the test pane

1. Select the **Test** icon in the upper-right of the page to open the **Test** pane.

1. If available, enable **Show reasoning** or **Show thinking steps** in the test pane options. This shows how the agent decides what to do before generating a response.

   > [!NOTE]
   > The reasoning trace is a key feature of the new experience. It shows which knowledge sources or actions the agent considered and why.

### Task 4.2 – Test the instructions

1. At the top of the **Test** pane, select the **Start new test session** icon **+**.

1. Enter the following prompt:

   ```prompt
   I forgot my password and cannot log in.
   ```

   Based on the instructions you wrote, the agent should direct you to the self-service portal before offering to raise a ticket.

### Task 4.3 – Test the action

1. At the top of the **Test** pane, select the **Start new test session** icon **+**.

1. Enter the following prompt:

   ```prompt
   My laptop will not turn on at all.
   ```

   Based on the instructions you wrote, the agent should decline to speculate about hardware failures and offer to contact the IT desk or submit a ticket.

1. Follow the agent's prompts to provide your name, email, and issue description.

   If the **Send an email** action is triggered, you may be prompted to authorize the connection. Select **Allow** if prompted.

### Task 4.4 – Refine instructions based on test results

1. Review how the agent responded across the three test sessions.

1. If any response was not aligned with the intended behavior, select **Edit** in the **Instructions** section and adjust the relevant guideline.

   For example, if the agent did not mention the self-service portal for password resets, make the instruction more explicit:

   ```prompt
   - For ALL password-related requests, always mention https://aka.ms/sspr as the first step before any other assistance.
   ```

1. Select **Save** and re-test the affected scenario.

   > [!NOTE]
   > Iterating on instructions is the primary tuning mechanism in the new experience. Small changes in wording can significantly change agent behavior.

## Summary

In this lab, you used the new Copilot Studio experience to create an instruction-driven IT support agent. You configured behavior entirely through natural language instructions and added a prebuilt connector action directly in Copilot Studio — without building a Power Automate workflow or leaving the page. Compared to Lab 03, where adding a tool required authoring a workflow, configuring inputs and outputs, and publishing it separately, the new experience significantly reduces the authoring effort for straightforward actions. You also used the reasoning trace in the test pane to observe how the agent decided what to do before generating a response. Having worked with both the classic and new experiences, you can now choose the right approach for each scenario: instruction-driven for open-ended conversations, and classic topics and workflows when you need a guaranteed, auditable sequence of steps.
