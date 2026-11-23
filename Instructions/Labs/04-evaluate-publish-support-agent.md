---
lab:
  title: Evaluate and publish a support agent
  module: Evaluate, publish, and manage agents in Microsoft Copilot Studio
  description: Connect a specialist agent, evaluate delegation, correct an instruction defect, publish the main agent, and validate its behavior in Microsoft Teams.
  duration: 55 minutes
  level: 200
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
    - Agents
---

# Evaluate, publish, and manage agents in Microsoft Copilot Studio

## Scenario

In this exercise, you create the **Contoso Policy Agent** and a specialized escalation agent. You connect the agents, evaluate delegation with a structured set of test conversations, correct one deliberate instruction defect, publish the main agent to Microsoft Teams, and validate its behavior as a published user.

This exercise takes approximately **55 minutes** to complete.

## What you learn

- How to create and run a repeatable agent evaluation.
- How to connect a specialist agent and test delegation boundaries.
- How to diagnose and correct an instruction defect.
- How to publish an agent to Microsoft Teams.
- How to validate agent behavior as a published user.

## High-level lab steps

- Create and configure a support policy agent and an escalation specialist.
- Connect the specialist to the main agent.
- Create and run an evaluation test set.
- Identify and correct an instruction defect.
- Rerun the evaluation and compare results.
- Publish and validate the agent in Microsoft Teams.

## Prerequisites

To complete this exercise, you need:

- A Microsoft Entra work or school account.
- [Access to Microsoft Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/requirements-licensing-subscriptions).
- Permission to create and publish two agents powered by the GitHub Copilot harness in the same Power Platform environment.
- Access to the **Evaluate** feature. Evaluate is in production-ready preview. Confirm with your instructor or administrator that it's enabled in your environment before you begin Exercise 2.
- A Microsoft Teams account to validate the agent after publishing.
- Permission to install the agent for yourself or access to an administrator who can approve the agent after you publish it.

> [!NOTE]
> Building, testing, and evaluating an agent powered by the GitHub Copilot harness may consume Copilot Credits. Check with your administrator if you're uncertain about your organization's credit balance before starting.

## Exercise 1 - Create the Contoso policy agent

### Task 1.1 - Create the agent

1. Go to [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) at `https://copilotstudio.microsoft.com/` and sign in with your work or school account.

1. Confirm that the correct environment is selected.

1. On the **Home** page, select **Agent**. Alternatively, select **Agents** in the left navigation and then select **New agent** in the upper-right corner.

1. In the **Name** field, enter the agent name:

   ```text
   Contoso Policy Agent <your initials>
   ```

   Replace `<your initials>` with your own initials or another short, unique identifier. For example: `Contoso Policy Agent CS`. Using your initials ensures the agent name is unique in a shared training environment.

1. If your organization requires a solution, in the upper-right corner, select **More options** (**...**), and then select **Settings**. In **Agent settings** > **Agent details**, check the **Solution** list. If the solution provided for the exercise isn't already selected, select it and then select **Done**. If it's already selected, you don't need to select **Done**; close the settings dialog.

1. Remain on the agent's **Build** page. In the next task, you configure the agent's instructions from this page.

### Task 1.2 - Add the agent instructions

> [!IMPORTANT]
> The instructions below contain a deliberate defect in the **Scope boundaries** section. Do not change the escalation instruction at this stage. You identify the defect in Exercise 3 and correct it in Exercise 4.

1. On the **Build** page, locate the **Instructions** section. Insert the text below:

   ```text
   You are the Contoso Policy Agent. You help Contoso employees find answers to questions about IT and HR support policies.

   ## Your role

   - Answer questions about Contoso IT support policies, HR support procedures, and help desk processes.
   - Provide accurate, complete answers based on the policy content in your instructions.
   - Keep responses professional, concise, and helpful.

   ## Scope boundaries

   - Only answer questions related to the Contoso IT and HR support policies listed in your instructions.
   - If a question is outside your scope, let the user know you can only help with Contoso IT and HR support topics.
   - If a user needs to speak with a person, let them know this is an automated assistant and that you cannot connect them directly to a human. Direct them to search the employee portal for contact information.

   ## Contoso support policies

   ### IT support

   **Password reset**
   Employees can reset their own password at https://aka.ms/sspr. If self-service password reset fails, contact the IT Help Desk. Self-service reset is available 24 hours a day, 7 days a week.

   **Hardware issues**
   Report hardware failures to the IT Help Desk within 24 hours of discovery. Include the device serial number and a description of the issue. Replacement or repair typically takes 3 to 5 business days.

   **Software requests**
   Submit all software installation requests through the IT Service Catalog. Standard software requests are approved within 2 business days. Licensed software requires manager approval and may take up to 10 business days.

   **Remote access**
   Contoso employees use the Contoso VPN client for remote access. Contact the IT Help Desk if you have trouble connecting. Remote access is available to all full-time employees and approved contractors.

   **IT Help Desk hours**
   The IT Help Desk is available Monday through Friday, 8:00 AM to 6:00 PM in your local time zone.

   ### HR support

   **Leave requests**
   Submit leave requests through the HR portal linked from the Contoso employee home page. Submit planned leave at least 5 business days in advance. Submit emergency leave on the day of absence with a brief reason.

   **Benefits questions**
   For benefits questions, contact the HR Benefits team at benefits@contoso.com or call the benefits hotline at extension 2840. Benefits enrollment is open each year during November.

   **Performance reviews**
   Contoso conducts performance reviews twice per year: in June and in December. Employees complete a self-evaluation in Workday before their review meeting.

   **New hire onboarding**
   New hires complete IT setup on their first day using the onboarding checklist in the employee portal. HR onboarding sessions are scheduled automatically during the first week.
   ```

1. After pasting the instructions, select **Save** to save the agent.

1. In the **Preview** tab, send the message `What are the IT Help Desk hours?` to confirm the agent responds. You should see a response that states Monday through Friday, 8 AM to 6 PM. If the agent doesn't respond or returns an error, verify the instructions are saved and that your environment supports agents powered by the GitHub Copilot harness.

### Task 1.3 - Create and publish the escalation specialist

1. Return to the **Agents** page, and then select **New agent**.

1. In the **Name** field, enter `Contoso Escalation Agent <your initials>`. Use the same unique identifier that you used for the policy agent.

1. In the **Instructions** section, insert the following text:

   ```text
   You are the Contoso Escalation Agent. You handle unresolved IT and HR support cases when an employee asks to escalate an issue or speak with a person.

   - For IT issues, direct the employee to call the IT Help Desk at extension 4357. The help desk is available Monday through Friday, 8:00 AM to 6:00 PM.
   - For HR matters, direct the employee to contact HR Support at hr-support@contoso.com or call the benefits and HR hotline at extension 2840.
   - Ask one clarifying question if you can't determine whether the issue is related to IT or HR.
   - Don't answer routine policy questions. Handle only escalation and human-support requests.
   ```

1. Select **Save**. On the **Preview** tab, send `I need to escalate an unresolved laptop ticket.` Confirm that the agent provides extension 4357.

1. On the **Build** page, select **Publish** > **Publish agent**. If publishing starts automatically, you don't need to select **Publish** a second time.. The specialist must be published before the policy agent can connect to it. When publishing is complete, select **Done**.

### Task 1.4 - Connect the escalation specialist

1. Return to the **Agents** page, and then open `Contoso Policy Agent <your initials>`.

1. On the **Build** tab, select **Connected agents** in the components panel.

1. Search for and select `Contoso Escalation Agent <your initials>`. If necessary, select **Refresh** to make the agent available for selection.

1. Use the following description:

   ```text
   Handles unresolved Contoso IT or HR support cases when an employee asks to escalate an issue or speak with a person. Provides approved human-support contact information. Do not use for routine policy questions.
   ```

1. Select **Connect**, and then confirm that the specialist appears under **Connected agents**.

The specialist's distinct description helps the main agent decide when to delegate. The deliberate instruction defect in the main agent still conflicts with this routing. You identify and correct the conflict after you run the baseline evaluation.

## Exercise 2 - Create evaluation conversations

> [!IMPORTANT]
> The **Evaluate** feature for agents powered by the GitHub Copilot harness is in production-ready preview. Confirm it's available in your environment before proceeding. If Evaluate is not enabled, skip to Exercise 4 and validate the agent using Preview and the published channel.

You create four evaluation conversations that cover the most important scenarios for the support policy agent: a direct answer retained by the main agent, an ambiguous request requiring clarification, an out-of-scope boundary, and an escalation request that should delegate to the connected agent.

### Task 2.1 - Configure a new test set

1. Select the **Evaluate** tab.

1. If prompted, review and accept any preview terms or notices.

1. Select **New evaluation** or **+ Create your first evaluation**.

1. In **Create or select a data set**, select **Conversations** as the data type, then select **Or, write some questions yourself**.

1. Enter `Contoso support release evaluation` for the evaluation name and confirm that **General quality** appears as the test method.

### Task 2.2 - Create conversation 1: In-scope answer

1. Select **+ Add conversations** > **Write a conversation**.

1. In the new row, select the empty field under **Conversation**, and then add the following user question:

   ```text
   What are the IT Help Desk hours?
   ```

1. Optionally, add this expected agent response:

    ```text
    The IT Help Desk is available Monday through Friday, 8 AM to 6 PM in your local time zone.
    ```

   > [!NOTE]
   > In this General quality evaluation, expected agent responses are for your own reference during manual review. General quality evaluates standards such as relevance and completeness. It doesn't compare the actual response against the expected response or verify connected-agent invocation. You configure an automated delegation check in Task 2.4.

1. Select **Done**, and then select **Save**.

### Task 2.3 - Create the remaining conversations

1. Repeat steps 1 through 4 from the previous task to add each conversation listed in the following table:

| Conversation | User question | Expected agent response |
| --- | --- | --- |
| Ambiguity and clarification | I need help with my benefits. | What would you like help with: benefits enrollment, leave requests, or another HR topic? |
| Out-of-scope boundary | Can you recommend a good restaurant near the office for a team lunch? | I can only help with Contoso IT and HR support topics, so I can't recommend a restaurant. |
| Connected-agent delegation | I submitted a ticket about my laptop three days ago, and nobody has responded. I need to speak with someone urgently. What should I do? | For urgent help with your laptop ticket, call the IT Help Desk at extension 4357. The help desk is available Monday through Friday, 8 AM to 6 PM. |

### Task 2.4 - Add an automated delegation check

1. Save **Contoso support release evaluation**, then repeat Task 2.1 to create a separate evaluation named `Contoso escalation delegation evaluation`.

1. Add only the **Connected-agent delegation** conversation from Task 2.3, including its expected agent response. Keep the other three conversations in the original evaluation; they don't require escalation.

1. In **Configure test set**, select **Add test method**, and then select **Capability use**. Keep **General quality** for response-quality scoring.

1. Configure the delegation conversation's expected capability to require invocation of `Contoso Escalation Agent <your initials>`, the specialist you connected in Task 1.4. Select that connected-agent capability, not a general capability or the policy agent itself, and save the evaluation.

   > [!IMPORTANT]
   > The expected capability is the automated assertion: the check should fail when the specialist isn't invoked, even if the main agent gives a plausible answer or mentions extension 4357. Expected response text alone doesn't assert delegation. If your preview environment doesn't expose the connected specialist as an expected capability, this check can't verify delegation there. Confirm availability with your instructor or administrator; don't treat a General quality score or a Keyword match on `4357` as proof of invocation.

## Exercise 3 - Run the baseline evaluation and observe the defect

### Task 3.1 - Run the evaluation

1. On the **Evaluate** page, open **Contoso support release evaluation** and confirm that all four conversations are listed.

1. Select **Run**.

1. If the **Manage connections** dialog opens, select the connection and confirm you are using your lab account. Then select **Continue**.

   > [!NOTE]
   > If the evaluation doesn't start, refresh the page, create a new connection and sign in to authenticate the account. Then try to run the evaluation again.

1. Wait for the evaluation run to complete. If the **evaluation is ready** notification appears, select **View results**. You can also open the completed run from the **Evaluations** list.

1. Open **Contoso escalation delegation evaluation** and run it as well. Wait for it to complete before changing the agent's instructions.

### Task 3.2 - Review baseline results

1. Open the completed baseline run of **Contoso support release evaluation**.

1. Review the General quality score for each conversation. Record your observations in the table below.

   | Conversation | Observed General quality result |
   | --- | --- |
   | IT Help Desk hours | |
   | Vague benefits question | |
   | Out-of-scope restaurant request | |
   | Unresolved ticket escalation | |

1. Open the baseline run of **Contoso escalation delegation evaluation**. Record the **Capability use** result for its delegation conversation. It should fail because the deliberate instruction defect prevents the expected specialist invocation. A high General quality score doesn't override this failure.

1. Select the delegation conversation to expand the detail view. Read the Capability use result and any explanation or invocation details to investigate the failed check.

1. Compare the actual response to the expected agent response. The defective agent might decline to connect the user to a person or direct them to search the employee portal. Even if it supplies extension 4357, the Capability use check should fail when it doesn't invoke the specialist.

   > [!NOTE]
   > Agent behavior can vary. If Capability use passes in the baseline, inspect the invocation details and verify that the original defective instruction is still saved. Record the observed result rather than assuming a failure.

### Task 3.3 - Identify the defect

1. Based on your review of the Capability use result and conversation details, identify the instruction that conflicts with escalation delegation.

1. Return to the **Build** tab and locate this statement in the **Scope boundaries** section of the agent's instructions:

   ```text
   If a user needs to speak with a person, let them know this is an automated assistant and that you cannot connect them directly to a human. Direct them to search the employee portal for contact information.
   ```

1. Notice that this instruction conflicts with the connected agent's purpose and prevents delegation to the escalation specialist. This instruction must allow the main agent to delegate matching requests.

## Exercise 4 - Correct the defect

### Task 4.1 - Update the agent instructions

1. Replace the escalation boundary statement you identified in the previous task with:

    ```text
    If a user asks to speak with a person or escalate an unresolved IT or HR issue, delegate the request to the connected escalation specialist. Don't block the request or attempt to provide escalation details yourself.
    ```

1. Select **Save**.

1. In the **Preview** tab, start a new conversation and send the message `I need to speak to someone about my laptop ticket.` Confirm that the main agent delegates the request and the response provides extension 4357.

1. Start another conversation and send `What are the IT Help Desk hours?` Confirm that the main agent answers the routine policy question without delegating it to the escalation specialist.

## Exercise 5 - Rerun the evaluation and compare results

### Task 5.1 - Run the evaluation again

1. Select **Evaluate** in the top navigation, open **Contoso support release evaluation**, and then select **Configuration**.

1. Confirm that your four conversations are still listed.

1. Select **Run** or **Run again**.

1. If the **Manage connections** dialog opens, select the connection and confirm you are using the account provided for the lab. Then select **Continue**.

1. Wait for the evaluation run to complete.

1. Open **Contoso escalation delegation evaluation**. Confirm that its conversation and required escalation-agent capability are unchanged, and then run it again. Wait for this run to complete.

### Task 5.2 - Compare results

1. If the results view is not displayed, select the latest evaluation run to open its results.

1. In **Contoso escalation delegation evaluation**, compare the **Capability use** result between the baseline run and the latest run. It should change from **Fail** to **Pass**, confirming that the expected escalation specialist was invoked. If it still fails, review the saved instruction and connected-agent configuration before proceeding.

1. Select the conversation detail to read the agent's updated response. Confirm that the connected escalation specialist provides the IT Help Desk extension 4357. Compare General quality scores as a separate response-quality measure; neither a high score nor contact information alone proves delegation.

1. Return to **Contoso support release evaluation**. Review all four conversations and confirm that the instruction change didn't reduce the other three conversations' General quality scores.

> [!NOTE]
> Copilot Studio saves each evaluation run separately. Compare the latest run with the earlier baseline run after you save the corrected agent.

## Exercise 6 - Add the Teams + Microsoft 365 channel and publish the agent

### Task 6.1 - Configure the Teams + Microsoft 365 channel and publish the agent

1. On the **Build** page, select **Publish**.

1. Select **+ Add channel**.

1. Select **Teams + Microsoft 365**.

1. Select **Microsoft Teams only**.

1. On the **About info** tab, complete the following fields, where available:

   | Field | Value |
   | --- | --- |
   | **Short description** | `Answers Contoso employee questions about IT and HR support policies.` |
   | **Long description** | `Use this agent to find answers about password resets, hardware issues, software requests, remote access, leave requests, benefits enrollment, and performance reviews.` |

1. Select **Add channel**.

1. Select **Publish agent**.

### Task 6.2 - Install the agent for yourself

Choose the path that matches your organization's policy.

**If your organization permits self-installation:**

1. When the publishing confirmation appears, select **View in Teams + Microsoft 365**.

1. In the browser prompt to open Microsoft Teams, select **Cancel**, select **Use the web app instead**. If prompted, sign in with your lab account.

1. In Microsoft Teams, select **Add**. This action installs the agent in Microsoft Teams when Microsoft 365 availability is enabled.

**If your organization requires administrator approval:**

1. In the components panel, under **Channels**, select **Teams + Microsoft 365**.

1. Select **Use and share**, and then select **Submit to org catalog** twice.

1. In the **Give everyone access to this agent?** dialog, select **Yes, submit** to send the agent for administrator approval.

1. Contact your Teams administrator and request approval. Provide the agent's display name and a brief description of its purpose.

1. Do not proceed to validation until the administrator confirms the agent is available in the catalog or is pre-installed for your account.

> [!NOTE]
> If tenant policy prevents installation and no administrator is available during the exercise, use Preview as the fallback validation path for the next task. Preview doesn't prove published-channel behavior. Record this limitation and complete channel validation with your administrator later.

## Exercise 7 - Validate as the published user

### Task 7.1 - Test the agent in Microsoft Teams

After the agent is installed by you or your administrator, open and test it from Microsoft Teams, not from the Copilot Studio authoring environment.

1. Open Microsoft Teams, and then locate and open the **Contoso Policy Agent** app you installed. Use the display name you configured. If the agent's dialog is still open, select **Open**.

1. Send the following messages one at a time and observe the responses:

   | Test message | Expected behavior |
   | --- | --- |
   | `What are the IT Help Desk hours?` | Agent states Monday through Friday, 8 AM to 6 PM local time |
   | `How do I request new software?` | Agent explains the IT Service Catalog process and approval timelines |
   | `Can you book a meeting room for me?` | Agent declines and states it can only help with Contoso IT and HR support topics |
   | `I've been waiting three days on a ticket. I need to talk to someone now.` | Main agent delegates to the escalation specialist, which provides IT Help Desk extension 4357 |

1. For each response, verify:

   - The response is accurate and matches the policy content in the agent's instructions.
   - The agent declines out-of-scope requests without attempting to answer them.
   - The main agent retains routine policy questions.
   - The escalation request is delegated and the response includes extension 4357.

1. If a response is incorrect or missing expected content, open **Monitor** in Copilot Studio to review the conversation and determine whether the issue is a quality defect, a permission failure, or a runtime error.

### Task 7.2 - Record validation results

In your lab notes, record the result of each validation test in the following table:

| Validation area | Result | Notes |
| --- | --- | --- |
| Authentication (agent loads without error) | Pass / Fail | |
| In-scope answer accuracy | Pass / Fail | |
| Out-of-scope decline | Pass / Fail | |
| Connected-agent delegation | Pass / Fail | |
| Routine request retained by main agent | Pass / Fail | |
| No unexpected errors in responses | Pass / Fail | |

## Completion criteria

You have completed this exercise when you can confirm all of the following:

- [ ] The Contoso Policy Agent is created with a unique name that includes your initials.
- [ ] The Contoso Escalation Agent is created, tested, and published in the same environment.
- [ ] The escalation specialist is connected to the policy agent with a distinct routing description.
- [ ] Four conversations are saved in Contoso support release evaluation.
- [ ] A separate delegation evaluation requires invocation of the connected escalation specialist through Capability use.
- [ ] Both baseline evaluations are complete, and you recorded the delegation conversation's Capability use result and investigated any unexpected pass.
- [ ] You can identify the specific instruction text that caused the escalation defect.
- [ ] The corrected instructions allow the main agent to delegate escalation requests to the connected specialist.
- [ ] The delegation evaluation rerun passes Capability use, and the response includes IT Help Desk extension 4357.
- [ ] The agent is published and the Teams + Microsoft 365 channel is added.
- [ ] You accessed the agent in Microsoft Teams, contacted your administrator for approval, or documented the fallback validation path.
- [ ] You validated in-scope answers, boundary behavior, and escalation behavior through the published Teams channel or the available fallback path.

## Clean up

After you complete the exercise, disconnect and delete both agents to avoid consuming Copilot Credits from future testing or accidental interactions.

1. In Copilot Studio, open `Contoso Policy Agent <your initials>`.

1. On the **Build** tab, select **Connected agents**. Open the escalation specialist, and then select **Disconnect**.

1. Locate your agent in the **Agents** list, and then select **More options** (**...**) in the upper-right corner.

1. From the menu, select **Delete agent**.

1. Enter the agent's exact name, and then select **Delete agent** to confirm the permanent deletion.

1. Repeat the deletion steps for `Contoso Escalation Agent <your initials>`.

If you created the agents within a solution, remove them from the solution before deleting them from the environment to ensure a complete cleanup.
