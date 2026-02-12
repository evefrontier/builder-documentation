# Environment Setup

To start building on Sui and EVE Frontier, follow the steps below to set up your local development tools. If you already have the tools, make sure they are the correct version to avoid difficulties running examples and building.

## Recommended: Docker (any OS)

The fastest way to get a Sui local development environment is with Docker. This works on Windows, Linux, and macOS with a single prerequisite.

**Prerequisite:** [Docker](https://docs.docker.com/get-docker/) installed and running.

Use the EVE Frontier builder-scaffold localnet setup:

1. Clone the builder-scaffold repo and navigate to the Docker setup:

```bash
git clone -b build https://github.com/evefrontier/builder-scaffold.git
cd builder-scaffold/localnet-setup/docker
```
> Note:  This link needs to will be updated soon
2. Follow the instructions in the [localnet-setup/docker](https://github.com/evefrontier/builder-scaffold/tree/build/localnet-setup/docker) directory.

This gives you a pre-configured Sui localnet and development environment without installing Sui CLI, WSL, or platform-specific tools.

---

## Manual setup by OS

If you prefer to install tools directly on your system, follow the steps for your OS below.

{% tabs %}

{% tab title="Windows" %}

This guide is for **Windows** users. Sui CLI installs via shell scripts, so you need WSL (Windows Subsystem for Linux). Run commands in PowerShell or Command Prompt with Admin privileges where noted.

## Step 1: Setup WSL

### Step 1.1: Install WSL

```bash
wsl --install
```

If you get an error about virtualization not being enabled, enable it through this [help article](https://support.microsoft.com/en-gb/windows/enable-virtualization-on-windows-c5578302-6e43-4b4b-a449-8ced115f58e1) and try again.

### Step 1.2: Update WSL

```bash
wsl --update
```

### Step 1.3: Install Ubuntu

```bash
wsl --install -d Ubuntu-24.04
```

### Step 1.4: Access WSL

```bash
wsl -d Ubuntu-24.04
```

## Step 2: Install Git

```bash
sudo apt-get install git curl
```

## Step 3: Install Sui CLI via suiup

[suiup](https://github.com/MystenLabs/suiup) is the recommended installer for Sui. It lets you install and switch between Sui CLI versions.

```bash
curl -sSfL https://raw.githubusercontent.com/MystenLabs/suiup/main/install.sh | sh
```

Restart your shell, then install Sui for testnet:

```bash
suiup install sui@testnet
```

Verify:

```bash
sui --version
```

## Step 4 (Optional): Node.js and PNPM

For interaction using typescript sdk:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash && . ~/.bashrc && nvm install 24
npm install -g pnpm
```

{% endtab %}

{% tab title="Linux" %}

This guide is for **Linux** users. Supported: Ubuntu 22.04 or newer.

## Step 1: Install Git

```bash
sudo apt-get install git curl
```

## Step 2: Install Sui CLI via suiup

[suiup](https://github.com/MystenLabs/suiup) is the recommended installer for Sui.

```bash
curl -sSfL https://raw.githubusercontent.com/MystenLabs/suiup/main/install.sh | sh
```

Restart your shell, then install Sui for testnet:

```bash
suiup install sui@testnet
```

Verify:

```bash
sui --version
```

## Step 3 (Optional): Node.js and PNPM

For interaction using typescript sdk:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash && . ~/.bashrc && nvm install 24
npm install -g pnpm
```

{% endtab %}

{% tab title="macOS" %}

This guide is for **macOS** users. Supported: macOS Tahoe or newer.

## Step 1: Install Homebrew (if needed)

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Add brew to your path:

```sh
export PATH="/opt/homebrew/bin:$PATH"
```

## Step 2: Install Git

```bash
brew install git
```

## Step 3: Install Sui CLI

**Option A — suiup (recommended):**

```bash
curl -sSfL https://raw.githubusercontent.com/MystenLabs/suiup/main/install.sh | sh
```

Restart your shell, then:

```bash
suiup install sui@testnet
```

**Option B — Homebrew:**

```bash
brew install sui
```

Verify:

```bash
sui --version
```

## Step 4 (Optional): Node.js and PNPM

For interaction using typescript sdk:

```sh
brew install node@24
npm install -g pnpm
```

{% endtab %}

{% endtabs %}

## Configure Sui Client

Installing Sui does not configure the client. To use `sui` commands:

1. [Configure the Sui client](https://docs.sui.io/guides/developer/getting-started/configure-sui-client) to create an address and connect to a network.
2. [Get SUI from the faucet](https://docs.sui.io/guides/developer/getting-started/get-coins) for testnet.

## Next Steps

You are ready to build on EVE Frontier. If you learn by doing, head to [Smart Assemblies](../smart-assemblies/storage-unit/README.md). To understand concepts first, see [Introduction to Smart Contracts](../smart-contracts/introduction-to-smart-contracts.md).
