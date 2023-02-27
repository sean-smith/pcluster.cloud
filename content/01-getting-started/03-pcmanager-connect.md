+++
title = "c. Connect to ParallelCluster UI"
weight = 13
+++

1. First navigate to the ParallelCluster UI. To do that, go to [**AWS CloudFormation**](https://console.aws.amazon.com/cloudformation/home) > **pcluster-ui** > **Outputs** then click on the **ParallelClusterUIURL** to connect.

{{% notice note %}}
To customize the URL please see [h. Setup Custom Domain 🔗](/02-tutorials/08-custom-domain.html).
{{% /notice %}}

![ParallelCluster UI Deployed](pcmanager-url.png)

2. During deployment you received an email titled **[ParallelClusterUI] Welcome to ParallelCluster UI, please verify your account.**. Copy the temporary password provided:

> From: "no-reply@verificationemail.com" <no-reply@verificationemail.com>
  Date: Monday, February 20, 2023 at 1:00 PM
  To: you@email.com
  Subject: [EXTERNAL] [AWS ParallelCluster UI] Welcome to AWS ParallelCluster UI, please verify your account.

> You are invited to manage clusters with ParallelCluster UI. Your administrator will contact you with the link to access. Your username is you@email.com and your temporary password is XXXXXX (you will need to change it in your first access).

3. **Enter the credentials**  using the *email* you used when deploying the stack and the *temporary password* from the email above.

![ParallelCluster UI CloudFormation Stack](pcmanager-creds.png)

4. You will be asked to provide a new password. Enter a new password to complete signup.

![Signup Screen](signup.png)

Congrats! You are ready to create your HPC cluster in AWS. Let's do that in the next section.
