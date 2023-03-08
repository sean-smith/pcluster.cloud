+++
title = "f. Slurm REST API 🌀"
weight = 26
+++

Enable the Slurm REST API. Requires Slurm Accounting.

### Step 1 - Setup Slurm Accounting

Slurm Accounting is required to enable the Slurm REST API. Follow these [instructions](/02-tutorials/02-slurm-accounting.html) to setup an Accounting database but **do not begin cluster creation** until completing Step 4.

### Step 2 - Create a Security Group to allow inbound API Requests

By default, your cluster will not be able to accept incoming HTTPS requests to the REST API. You will need to [create a security group](https://console.aws.amazon.com/ec2/v2/home?#CreateSecurityGroup:) to allow traffic from outside the cluster to call the API.

1. Under **Security group name**, enter `Slurm REST API` (or another name of your choosing)
2. Ensure **VPC** matches the cluster's VPC
3. Add an inbound rule and select `HTTPS` under `Type`, then change the `Destination` to the CIDR range you want to have access. In this example we use `Anywhere-IPv4` but you should restrict this down in practice.
4. Click **Create security group**

    ![Security Group](/02-tutorials/06-slurm-rest-api/security-group.png)

### Step 3 - Add Additional IAM Permissions

Please follow the instructions under [g. Setup IAM Permissions 🔑](/02-tutorials/07-setup-iam.html). This step is only required if you're using ParallelCluster UI to setup your cluster.

### Step 4 - Configure your cluster

1. In your cluster configuration, return to the HeadNode section and add **Slurm REST API** Security Group you created above.

    ![Security Group](/02-tutorials/06-slurm-rest-api/add-security-group.jpeg)

2. Under `Advanced options` >  click `Add Script` and paste in:

    ```bash
    https://raw.githubusercontent.com/sean-smith/pcluster.cloud/main/static/scripts/rest-api.sh
    ```

    ![Security Group](/02-tutorials/06-slurm-rest-api/post-install.jpeg)

3. Under **Additional IAM permissions**, add the policy:

    ```bash
    arn:aws:iam::aws:policy/SecretsManagerReadWrite
    ```

    ![Cluster Setup](/02-tutorials/06-slurm-rest-api/iam-policy.jpeg)

4. Setup the rest of the options following the [accounting tutorial](/02-tutorials/02-slurm-accounting.html).

5. Create your cluster.

### Step 5 - Test

In order to authenticate with the API, you'll need a JWT token. One was created for you and stored in [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/listsecrets?#) with the name `slurm_token_[your-cluster]`. In the script below we fetch this using the AWS CLI.

You'll also need the HeadNode ipv4 address. This can be retrieved either manually through the ParallelCluster UI as shown below or automatically using the **pcluster cli**.

![Public ipv4 address](/02-tutorials/06-slurm-rest-api/public-ipv4.png)

Next run the following python or bash script, changing `cluster_name=rest-api` and `region=us-east-2` to match your cluster:

{{< tabs >}}
{{% tab name="python" %}}
```python
#!/usr/bin/python3
import boto3
import requests

headnode_ip = '18.220.163.141'
cluster_name = 'rest-api'
region = 'us-east-2'

client = boto3.client('secretsmanager', region_name=region)
jwt_token = client.get_secret_value(SecretId=f"slurm_token_{cluster_name}")

headers = {'X-SLURM-USER-NAME': 'ec2-user', 'X-SLURM-USER-TOKEN': jwt_token.get('SecretString')}

r = requests.get(f"https://{headnode_ip}/slurm/v0.0.36/diag", headers=headers, verify=False)

print(r.text)
```
{{% /tab %}}
{{% tab name="Bash" %}}
```bash
# set the cluster name and region:
cluster_name=rest-api
region=us-east-2

# this will get the headnode ip and JWT token
HEADNODE_IP=$(pcluster describe-cluster --region ${region} -n ${cluster_name} | jq '.headNode.publicIpAddress' | tr -d '"')
SLURM_JWT=$(aws --region ${region} secretsmanager get-secret-value --secret-id slurm_token_${cluster_name} --query SecretString --output text)

# call the api
curl -k -H "X-SLURM-USER-NAME:ec2-user" -H "X-SLURM-USER-TOKEN:$SLURM_JWT" https://$HEADNODE_IP/slurm/v0.0.36/diag
```
{{% /tab %}}
{{< /tabs >}}

You'll get a response back like:

  ```json
    {
      "meta": {
        "plugin": {
          "type": "openapi\/v0.0.36",
          "name": "REST v0.0.36"
        },
        "Slurm": {
          "version": {
            "major": 22,
            "micro": 8,
            "minor": 5
          },
          "release": "22.05.8"
        }
  ```

Congrats! You just called the Slurm API on the HeadNode. For other API endpoints see [Slurm REST API Reference](https://slurm.schedmd.com/rest_api.html).