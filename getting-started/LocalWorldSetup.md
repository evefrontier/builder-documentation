# Setting Up The World

![](./images/banner4.png)

import { Steps } from "nextra/components"
import { Callout } from "nextra/components"
import { Tabs, Tab } from "nextra/components"

## Install via Docker
To quickly setup the world, you need to use [Docker](https://www.docker.com/). Follow this guide to setup a local instance of the world.

<Steps>

### Step 1 - Follow the [Tools Guide](/Tools)
If you haven't already, follow the [Tools Guide](/Tools) to setup your tools and get the Builder Examples.

### Step 2 - Install [Docker](https://docs.docker.com/get-docker/)

<Tabs items={["Windows", "Linux", "MacOS" ]}>
 
  <Tab>
  Install and Setup Docker on **Windows** using these steps: 
  <Steps>
    ### Step 2.1 - Install Docker Desktop from Docker Hub for [Windows](https://docs.docker.com/desktop/install/windows-install/).
    Install docker using [this link](https://docs.docker.com/desktop/install/windows-install/).

    ### Step 2.2 - Start Docker.
    Start Docker Desktop through the start menu.

    ### Step 2.3 - Open the settings using the cog in the top right.
    In the top right near the search bar select the settings cog.

    ### Step 2.4 - Enable WSL
    Visit <strong>Resources > WSL Integration</strong> and Enable Integration with WSL as shown in the image below.
    <br/ >
    ![](./images/docker-wsl-2.png)

    ### Step 2.5 - Select Ubuntu
    Make sure you enable the Ubuntu integration in the image from <strong>step 4</strong>.
  </Steps>
  </Tab>
  <Tab>
  Install Docker Desktop from Docker Hub for [Linux](https://docs.docker.com/desktop/install/linux-install/) and start it.
  </Tab>
  <Tab>
    Install and Setup Docker on **MacOS** using these steps: 

    <Steps>
    ### Step 2.1 - Install Rosetta
    Install Rosetta to ensure Docker Desktop works on MacOS using:

    ```bash copy
    softwareupdate --install-rosetta
    ``` 

    ### Step 2.2 - Install Docker Desktop 
    Install Docker Desktop from Docker Hub for [Mac](https://docs.docker.com/desktop/install/mac-install/).

    ### Step 2.3 - Start Docker.
    Start Docker Desktop through the start menu.
    </Steps>
  </Tab>

</Tabs>

### Step 3 - Navigate to the builder examples folder

Navigate to your install of the Builder Examples through the terminal:

```bash copy
cd builder-examples
```

### Step 4 - Start the local world

Start the world deployer and local chain with:

```bash copy
docker compose up -d && docker compose logs -f world-deployer
```
  
<Callout emoji="💡">
This will take a few minutes and is complete when the Progress in the terminal displays 100%.
![](./images/docker-terminal-world-deployed.png)
Once this is complete the World Deployer container will go offline.
</Callout>
The world-deployer lines are the addresses for your world and you'll need them later when connecting to it, so keep them somewhere handy (_else you'll have to dig for them in the Containers logs later_).
</Steps>
**Congratulations**, you now have your local world setup up and running!

<Callout emoji="💡">
TIP: _you always have on overview of what is running through **Docker - Containers** and if you click the **world-deployer** you'll see the logs (including the world info when it was started in Step 4) or perform other actions (i.e. start or stop)..._
</Callout>

## Next Steps
If you already know the [MUD framework](https://mud.dev/), the Smart Object Framework, and the details of the EVE World (or you simply learn better by doing), you can skip straight to the builder examples [here](/SmartStorageUnit).