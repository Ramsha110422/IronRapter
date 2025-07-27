# Deploying and Running DeepSeek-R1 1.5B on Huawei Cloud ECS Using Ollama
With the rapid advancements in large language models (LLMs), developers are increasingly seeking efficient and scalable ways to deploy these models for experimentation and development. Huawei Cloud offers a powerful platform to achieve this. This guide walks through deploying the DeepSeek-R1 1.5B model using Ollama, a lightweight tool for managing and running LLMs locally on a cloud-based Elastic Cloud Server (ECS).

## Step 1: Preparing the Lab Environment
### 1.1 Creating an ECS Instance
To begin, access the Huawei Cloud console using the exercise-provided IAM credentials. Once logged in:

Navigate to Elastic Cloud Server (ECS) from the service list or search bar.

Click Buy ECS, select the Custom Config tab, and set the following:

Billing Mode: Pay-per-use

Region: AP-Singapore

Architecture: x86

Flavor: c6.xlarge.2 (4 vCPUs | 8 GiB)

Image: CentOS 8.2 64bit (40 GiB system disk)

Configure the Network:

VPC: Default (or create a new one if unavailable)

Security Group: Default (or create a new one if unavailable)

Public Network Access:

EIP: Auto assign

EIP Type: Dynamic BGP

Billing: Traffic-based

Bandwidth: 100 Mbps (Recommended for stable downloads)

Instance Name: ecs-cent
Password: Huawei1234% (Note: Must use exactly as specified)

Click Submit and then Agree and Submit to complete instance creation.

After successful setup, the ECS instance will appear on the ECS homepage.

### 1.2 Logging into the ECS
Use CloudShell to remotely access your ECS instance.

Enter the password Huawei1234% when prompted.

Once connected, proceed to install Ollama.

## Step 2: Installing Ollama
Ollama simplifies local deployment and interaction with LLMs. It supports models like LLaMA, Mistral, and DeepSeek, and is highly recommended for developers.

There are two ways to install Ollama:

### Solution 1: Install via GitHub
Run the following command to download and install Ollama from the official source:

bash
Copy
Edit
curl -fsSL https://ollama.com/install.sh | sh
To verify installation:

bash
Copy
Edit
ollama -v
### Solution 2: Install from OBS (Huawei Cloud)
For a faster installation using a pre-downloaded version (dated Feb 28, 2025):

bash
Copy
Edit
wget https://sandbox-experiment-files.obs.cn-north-4.myhuaweicloud.com/deepseek/ollama-linux-amd64.tgz
sudo tar -C /usr -xzf ollama-linux-amd64.tgz
sudo chmod +x /usr/bin/ollama
Create Ollama user if not found:

bash
Copy
Edit
id ollama
sudo useradd -r -s /bin/false -m -d /usr/share/ollama ollama
Create the service file:

bash
Copy
Edit
cat << EOF > /etc/systemd/system/ollama.service
[Unit]
Description=Ollama Service
After=network-online.target

# [Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3

# [Install]
WantedBy=default.target
EOF
Then reload and enable the service:

bash
Copy
Edit
sudo systemctl daemon-reload
sudo systemctl enable ollama
sudo systemctl start ollama
ollama -v
Step 3: Deploying DeepSeek-R1 1.5B
You can now deploy the DeepSeek-R1 1.5B model using one of two solutions:

### Solution 1: Pull Directly via Ollama
Use this command to pull the model from Ollama's registry:

bash
Copy
Edit
ollama pull deepseek-r1:1.5b
Once downloaded, run the model:

bash
Copy
Edit
ollama run deepseek-r1:1.5b
Tip: If download fails or is slow, use Ctrl+C to stop and rerun the command—Ollama supports resumable downloads.

### Solution 2: Download from OBS
For faster access using Huawei’s backup model version (dated Feb 14, 2025):

bash
Copy
Edit
wget https://sandbox-experiment-files.obs.cn-north-4.myhuaweicloud.com/deepseek/ollama_deepseek_r1_1.5b.tar.gz
sudo tar -C /usr/share/ollama/.ollama/models -xzf ollama_deepseek_r1_1.5b.tar.gz
Move files to the appropriate directories:

bash
Copy
Edit
cd /usr/share/ollama/.ollama/models
mv ./deepseek/sha256* ./blobs
mkdir -p ./manifests/registry.ollama.ai/library/deepseek-r1
mv ./deepseek/1.5b ./manifests/registry.ollama.ai/library/deepseek-r1
rm -rf deepseek/
Check the model list:

bash
Copy
Edit
ollama list
Run the model:

bash
Copy
Edit
ollama run deepseek-r1:1.5b
![WhatsApp Image 2025-07-27 at 6 43 40 PM](https://github.com/user-attachments/assets/dab39931-eda1-4b22-af41-beecca9f1ab0)

