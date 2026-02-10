# Setting Up Your Tools

## Installing

import { Steps } from "nextra/components" import { Tabs, Tab } from "nextra/components"

import { Callout } from "nextra/components";

To start building, follow the steps below to setup your local development tools and environment. If you already have the tools, make sure they are the correct version as otherwise you may have difficulties running the examples and building.

\<Tabs items={\["Windows", "Linux", "MacOS" ]}> This guide is for **Windows** users. Make sure you are running these commands in PowerShell or command prompt with Admin Privileges.

### Step 1: Setup WSL

#### Step 1.1: Install WSL

Install WSL is the Windows Subsystem for Linux. It allows you to run Linux commands on your Windows machine. Install it with:

```bash
wsl --install
```

If you get an error about virtualization not being enabled, enable it through this \[help article]\(https://support.microsoft.com/en-gb/windows/enable-virtualization-on-windows-c5578302-6e43-4b4b-a449-8ced115f58e1) and try to run the install command again.

#### Step 1.2: Update WSL

Update WSL to the latest version with:

```bash
wsl --update
```

#### Step 1.3: Install Ubuntu

Install Ubuntu on WSL using:

```bash
wsl --install -d Ubuntu-24.04
```

#### Step 1.4: Access WSL

To access Ubuntu on WSL you can use:

```bash
wsl -d Ubuntu-24.04
```

### Step 2: Change Directory

Once you're in the Ubuntu instance through WSL, you can get better performance by changing directory from the host directory to the Linux home directory with:

```bash
cd ~/
```

### Step 3: Open File Explorer (Optional)

To access the files from your Windows machine you can run this command:

```bash
explorer.exe .
```

### Step 4: Installing general tools

Before you get started you need to either install, or make sure you have the required tools.

#### Step 4.1: Git

Install Git and curl with:

```bash
sudo apt-get install git curl
```

#### Step 4.2: NVM + Node.JS

Install NVM and version 20 of Node.JS by using this command:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash && . ~/.bashrc && nvm install 20
```

If you already have Node.JS installed, you can use \`\`\`nvm use 20\`\`\` to use Node.JS Version 20.

#### Step 4.3: PNPM

Install PNPM, which is used as a more efficient version of NPM with:

```bash
npm install -g pnpm
```

### Step 5: Foundry & Forge

Install foundry, restart the shell and install forge with:

```bash
curl -L https://foundry.paradigm.xyz | bash && . ~/.bashrc && foundryup
```

### Step 6: Fetch the examples

Fetch the Builder examples with:

```bash
git clone https://github.com/projectawakening/builder-examples.git
```

This guide is for **Linux** users.

{

#### Step 1: Installing general tools

} Before you get started you need to either install, or make sure you have the required tools.

{

**Step 1.1: Git**

} Install Git and curl with:

```bash
sudo apt-get install git curl
```

{

**Step 1.2: NVM + Node.JS**

} Install NVM and version 20 of Node.JS by using this command:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash && . ~/.bashrc && nvm install 20
```

If you already have Node.JS installed, you can use \`\`\`nvm use 20\`\`\` to use Node.JS Version 20.

{

**Step 1.3: PNPM**

} Install PNPM, which is used as a more efficient version of NPM with:

```bash
npm install -g pnpm
```

{

#### Step 2: Foundry & Forge

} Install foundry, restart the shell and install forge with:

```bash
curl -L https://foundry.paradigm.xyz | bash && . ~/.bashrc && foundryup
```

{

#### Step 3: Fetch the examples

} Fetch the Builder examples with:

```bash
git clone https://github.com/projectawakening/builder-examples.git
```

This guide is for **MacOS** users.

{

#### Step 1: Installing general tools

} Before you get started you need to either install, or make sure you have the required tools.

{

**Step 1.1: Installing Homebrew**

} Homebrew is a package manager for MacOS, which will allow you to install the required tools. Install it using:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Then add brew to the path:

```sh
export PATH="/opt/homebrew/bin:$PATH"
```

{

**Step 1.2: Installing Git**

} Install Git with:

```bash
brew install git
```

{

**Step 1.3: Installing Node.JS**

} Install version 20 of NPM (Node.JS Version Manager) using Homebrew with:

```sh
brew install node@20
```

{

**Step 1.4: Installing PNPM**

} Install PNPM, which is used as a more efficient version of NPM with:

```bash
npm install -g pnpm@latest-8 && source ~/.zshenv
```

{

#### Step 2: Installing Foundry + Forge

} Install foundry, restart the shell and install forge with:

```bash
curl -L https://foundry.paradigm.xyz | bash && source ~/.zshenv && foundryup
```

{

#### Step 3: Fetch the examples

} Fetch the Builder examples with:

```bash
git clone https://github.com/projectawakening/builder-examples.git
```

## Next Steps

Now you are ready to setup the local world and chain on [Setting Up The World](https://github.com/evefrontier/builder-documentation/blob/Main/LocalWorldSetup/README.md).
