# Environment Setup

To start building on EVE Frontier, follow the steps below to set up your local development tools. If you already have the tools, make sure they are the correct version to avoid difficulties building.

## Community tool: efctl (one-command setup)

The community CLI **[efctl](https://frontier.scetrov.live/links/efctl/)** automates the full local setup.

---

## Recommended: Docker (any OS)

The fastest way to get a Sui local development environment is with Docker. This works on Windows, Linux, and macOS with a single prerequisite.

**Prerequisite:** [Docker](https://docs.docker.com/get-docker/) installed and running.

Use the EVE Frontier builder-scaffold localnet setup:

1. Clone the builder-scaffold repo and navigate to the Docker setup:

```bash
mkdir -p workspace && cd workspace
git clone https://github.com/evefrontier/builder-scaffold.git
cd builder-scaffold/docker
```

2. Follow the instructions in the [localnet-setup/docker](https://github.com/evefrontier/builder-scaffold/blob/main/docker/readme.md) directory.

This gives you a pre-configured Sui localnet and development environment without installing Sui CLI, WSL, or platform-specific tools.

---

## Manual setup by OS

If you prefer to install tools directly on your system, follow the steps for your OS below.

{% tabs %}

{% tab title="Windows" %}

This guide is for **Windows** users. [suiup](https://github.com/MystenLabs/suiup) is a native Windows executable. Run commands in PowerShell, Command Prompt, or **Git Bash** (recommended if you prefer using the bash install script).

## Step 1: Install Git

Install [Git for Windows](https://git-scm.com/download/win) if you haven't already. Git Bash (included with the install) lets you run the suiup bash install script directly without needing to download the binary manually:

```bash
curl -sSfL https://raw.githubusercontent.com/MystenLabs/suiup/main/install.sh | sh
```

Then continue from Step 3 below. If you prefer the manual binary install, follow Steps 2–3 instead.

## Step 2: Install suiup

suiup is the recommended way to install and manage Sui CLI. It is a cross-compiled native executable for Windows.

1. **Download** the latest Windows release from [suiup releases](https://github.com/MystenLabs/suiup/releases). Choose the archive for Windows x86_64 (e.g. `suiup-vX.X.X-windows-x86_64.zip`).

2. **Unzip** the archive. You'll get a folder containing the `suiup.exe` executable.

3. **Add suiup to your PATH** — choose one:

   **Option A — Use a dedicated bin folder (recommended)**

   - Create `%LOCALAPPDATA%\bin` if it doesn't exist (on most systems this is `C:\Users\<YourUsername>\AppData\Local\bin`).
   - Move `suiup.exe` into that folder.
   - Add this folder to your user PATH in Environment Variables:

   **Option B — Use the extracted folder**

   - Leave `suiup.exe` in the folder where you unzipped it.
   - Add that folder to your user PATH using the same steps as Option A (Environment Variables → Path → New).

4. **Restart** your terminal (PowerShell or Command Prompt) so the updated PATH is picked up. Test with `suiup --version`.

## Step 3: Install Sui CLI

```powershell
suiup install sui@testnet
```

Verify:

```powershell
sui --version
```

suiup installs binaries to `%LOCALAPPDATA%\bin`. Ensure this folder is on your `PATH`.

## Step 4 (Optional): Node.js and PNPM

For interaction using the TypeScript SDK, install [Node.js](https://nodejs.org/) (LTS) and then:

```powershell
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
