---
lab:
  title: ILT Setup
  module: Introduction
  description: In this exercise, you will access the Microsoft Copilot Studio portal and create an environment and solution to use throughout the remaining labs.
  duration: 10 minutes
  level: 200
  islab: true
  primarytopics:
    - Microsoft Copilot
    - Microsoft Copilot Studio
---
# ILT Setup

## Prerequisites

- An account with access to Microsoft Copilot Studio and a license that provides [Managed Environments use rights](https://learn.microsoft.com/en-us/power-platform/admin/managed-environment-licensing). Eligible trial licenses can be used, subject to their restrictions. The Power Apps Developer Plan alone does not provide Managed Environments use rights.
- A Power Platform Administrator or Dynamics 365 Administrator to create the managed developer environment. If your lab account does not have this role, ask your instructor or tenant administrator to provision the environment on your behalf and ensure you have access to its Dataverse database.

> [!IMPORTANT]
> [Unmanaged developer environments are deprecated](https://learn.microsoft.com/en-us/power-platform/admin/environments-overview). Use a **Developer** environment with **Managed Environments** enabled for these labs, not an unmanaged developer environment. If you already have an unmanaged developer environment, ask your administrator to [enable Managed Environments](https://learn.microsoft.com/en-us/power-platform/admin/managed-environment-enable) before using it.

If your instructor has already provided a managed developer environment with Dataverse, go directly to the Copilot Studio sign-in step in Task 1.3.

## Exercise 1 - Create a Power Platform environment

### Task 1.1 - Power Platform Admin Center

Before you start the lab exercises, you or your administrator must create a managed developer environment for you to work in.

1. Open a web browser, navigate to `https://admin.powerplatform.microsoft.com/manage/environments`, and sign in using the administrator account that will create the environment.

1. If prompted, choose the option to stay signed in.

1. Close any pop-up messages that are displayed.

### Task 1.2 - Add Dataverse to the default environment

1. Select the ellipsis (**...**) for the **default** environment (for example, **Contoso (default)**) and select **Add Dataverse**.

   ![Add Dataverse to the default environment in the Power Platform Admin center.](../media/add-dataverse.png)

1. Leave all of the default settings and select **Add**.

### Task 1.3 - Create a new environment

1. In the **Environments** page, select **+ New** to create a new environment with the following settings:

   - **Type**: Developer
   - **Macro Region Geography**: The region specified by your instructor (for example, Europe or North America)
   - **Name**: *Your name*

1. Expand **Change default settings** and configure the following:
   - **Environment group**: None
   - **Make this a Managed Environment**: Yes
   - **Create on behalf**: No if creating your own environment. If an administrator is creating it for a learner, select **Yes** and specify the learner as the owner.
   - **Add a Dataverse data store?**: Yes
   
   > [!NOTE]
   > If you cannot create a Developer environment or enable Managed Environments, ask your instructor or Power Platform administrator to provide a managed developer environment, for example through [environment routing](https://learn.microsoft.com/en-us/power-platform/admin/default-environment-routing). Do not create an unmanaged developer environment.

1. Select **Next** and in the **Add Dataverse** section:

   - **Language**: English (United States)
   - **Currency**: USD ($)
   - **Deploy sample apps and data?**: No

1. Select **Save** and wait until the state of your environment is **Ready** (you can use the **Refresh** button to update the display).

   > [!NOTE]
   > Environment provisioning can take several minutes depending on tenant configuration.

   ![Environment created in the Power Platform Admin center.](../media/environment-created.png)

1. Open the environment details and verify that the environment type is **Developer**, **Managed Environments** is enabled, and the Dataverse database has been provisioned. If an administrator created the environment on your behalf, confirm that you can access it using your learner account.

1. In a new browser tab, navigate to `https://copilotstudio.microsoft.com/` and sign in using your learner account.

   > [!NOTE]  
   > If you experience issues loading Copilot Studio on your environment:
   > - First, capture your environment ID (GUID) from the Power Platform admin center:
   >   1. Open the environment you created at `https://admin.powerplatform.microsoft.com/manage/environments`.
   >   2. Locate the environment ID in the URL (a long string such as `12345678-90ab-cdef-1234-567890abcdef`).
   >   3. Copy and save this value.
   > - Then try accessing your environment directly by pasting your ID into the following URL:
   >   ```
   >   https://copilotstudio.microsoft.com/environments/<your-environment-id>/home
   >   ```

1. If prompted with a **Get Started** screen, accept the default country or region and continue. Skip any welcome messages.

1. In the bottom-left corner, select the **environment selector** that displays the name of the currently selected environment, and then choose the managed developer environment prepared for these labs.

   ![Select your environment in the Copilot Studio.](../media/select-environment.png)

### Task 1.4 - Create a solution

1. In the left navigation pane, select the ellipsis (**...**) next to your account name and then select **Solutions**.

   > [!NOTE]
   > Selecting **Solutions** may redirect you to the classic experience. If this occurs, close the classic experience. Then, select the ellipsis (**...**) next to your account name again and select **Solutions** a second time to open the solutions list.

1. If prompted with a **Get Started** screen, select **Get Started** and then skip any welcome messages.

1. You should see several solutions including the *Default Solution* and the *Common Data Services Default Solution*.

   ![List of solutions in Maker portal.](../media/solutions-list.png)

1. Select **+ New solution**.

1. In the **Display name** text box, enter **`Lab Exercises`**

1. Verify that **Name** is automatically populated.

1. Select **+ New publisher** below the **Publisher** drop-down.

1. For **Display name**, enter `Fabrikam`

1. For **Name**, enter `fabrikam`

1. For **Prefix**, enter `fab`

1. Select **Save** to create the publisher.

   > [!NOTE]
   > If **Save** is unavailable, create the solution in [Power Apps](https://make.powerapps.com/) at `https://make.powerapps.com`.

1. Verify that **Fabrikam (fabrikam)** is selected in the **Publisher** drop-down.

1. Select the **Set as your preferred solution** checkbox.

   > [!NOTE]
   > Setting this as your preferred solution ensures new assets created during later labs are added to the Lab Exercises solution by default.

   ![New solution.](../media/new-solution.png)

1. Select **Create**.

1. Close the **Solutions** browser tab.

1. Refresh the **Copilot Studio** page.

You now have a Power Platform environment and solution to work in.
