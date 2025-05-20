# Project F

Nockchain

CLI Miner Setup
Hardware Requirements
Hardware requirement is highly speculative since no one knows how Mainnet-launch goes.

Miner is CPU-based initially
RAM	CPU	Disk
64 GB	6 cores or higher	100-200 GB SSD
more CPU = more hashrate = more chances
We still can't say that's enough, because it's just a testnet environment and we need to wait until mainnet.
OS Requirements
Windows Users: Install Linux Ubuntu on your Windows using this Guide
VPS: Recommended crypto-payment VPS provider to Purchase or Visit VPS Beginner Guide
Note: Miners are initially CPU-based for users and will eventually move to GPU and ASIC.

Zorp as a for-profit labs company behind Nockchain will be selling private, closed-source software to mining partners, possibly even GPU-based to help bootstrap the initial security and proofrate of the network.

Meaning they will likely dominate block rewards over the vast majority of people, but yet the protocol might potentially be rewarding for early users.

CLI Installation Steps
Step 1: Install Dependecies
Update Packages
sudo apt-get update && sudo apt-get upgrade -y
Install Packages:
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
Rust:
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
Docker (For Mainnet, we might choose Docker setup)
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world

sudo systemctl enable docker
sudo systemctl restart docker
Step 2: Clone NockChain Repo
git clone https://github.com/zorp-corp/nockchain

cd nockchain
Step 3: Install Choo (Jock/Hoon Compiler)
make install-hoonc
This compiles hoonc, the Nock-based compiler used for running Jock programs and ZKVM applications.
Step 4: Build
Building may take more than 15 minutes.

# Install node binaries
make build

# Install wallet binaries
make install-nockchain-wallet

# Install Nockchain
make install-nockchain
Step 5: Setup Wallet
Make sure you are in nockchain directory.

Set PATH:
export PATH="$PATH:$(pwd)/target/release"
Create wallet:
nockchain-wallet keygen
Save memo, private key & public key of your wallet.
Note: After every terminal restart, Ensure you execute these two commands before executing wallet commands again: cd nockchain & export PATH="$PATH:$(pwd)/target/release". By doing this, you won't get Error: wallet: command not found.

Step 6: Configure Nodes
Your Node's configuration is in Makefile Open Makefile:

nano Makefile
MINING_PUBKEY: Replace your wallet public key with its value.
Ports: By default, Nodes use ports 3005 and 3006. If these ports are occupied on your system, modify them in the node configuration.
To save: Ctrl+X + Y + Enter
Step 7: Open ports
# Allow ssh port
sudo ufw allow ssh
sudo ufw allow 22

# Enable firewall
sudoufw enable

# Open ports
sudo ufw allow 3005/tcp
sudo ufw allow 3006/tcp
Step 8: Run Leader Node
Leader Node is a fake testnet node. On mainnet Peer IDs will be replaced with Leader node, we only need to run Follower Node in the next step.

Open a screen:
screen -S leader
Start a Leader Node (Testnet Node for fake genesis block):
make run-nockchain-leader
Wait for it to install.
To minimize: Ctrl + A + D
Step 9: Run Follower Node:
Open a screen
screen -S follower
Start a Follower Node (Miner Node for connecting to other peers):
make run-nockchain-follower
# OK Response:
I (12:18:31) "inner dumbnet cause: [%command %timer]"
I (12:18:32) nc: block by-height: [Ok(%heavy-n) Ok(1) 0]
To minimize: Ctrl + A + D
