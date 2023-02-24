+++
title = "c. Connect to ParallelCluster UI"
weight = 13
+++

1. During deployment you received an email titled **[ParallelClusterUI] Welcome to ParallelCluster UI, please verify your account.**. Login with the temporary code provided.

> From: "no-reply@verificationemail.com" <no-reply@verificationemail.com>
  Date: Monday, February 20, 2023 at 1:00 PM
  To: you@email.com
  Subject: [EXTERNAL] [AWS ParallelCluster UI] Welcome to AWS ParallelCluster UI, please verify your account.

> You are invited to manage clusters with ParallelCluster UI. Your administrator will contact you with the link to access. Your username is you@email.com and your temporary password is XXXXXX (you will need to change it in your first access).

2. **Enter the credentials**  using the *email* you used when deploying the stack and the *temporary password* from the email above.

![ParallelCluster UI CloudFormation Stack](pcmanager-creds.png)

3. You will be asked to provide a new password. Enter a new password to complete signup.

![Signup Screen](signup.png)

Congrats! You are ready to create your HPC cluster in AWS. Let's do that in the next section.

{{% notice note %}}
To get the URL outside of the email, go to [**AWS CloudFormation**](https://console.aws.amazon.com/cloudformation/home) > **pcluster-ui** > **Outputs** then click on the **ParallelClusterUIURL** to connect.
![ParallelCluster UI Deployed](pcmanager-url.png)
{{% /notice %}}