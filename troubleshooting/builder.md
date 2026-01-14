# Builder

{% tabs %}
{% tab title="Windows / WSL" %}
Common issues and solutions for **Windows using WSL**.

#### Error: Could Not Find World Address

When deploying smart contracts make sure that you are using the correct world address. This is set in the .env for the contracts.

For local worlds the World Address should be:

```bash
0x0165878a594ca255338adfa4d48449f69242eb8f
```

And for Stillness the World Address should be:

```bash
0xcdb380e0cd3949caf70c45c67079f2e27a77fc47
```

The deploy command should look something like this:

```bash
pnpm deploy
```

#### Environment Variables Missing

If your environment variables are empty, you can restore them by loading them from the environment file with:

```bash
export $(grep -v '^#' .env | xargs)
```

#### Using Windows to edit Environment Variables

Firstly, replace the line endings in the environment file using:

```bash
sed -i 's/\r$//' .env
```

Then, reload the environment file using:

```bash
export $(grep -v '^#' .env | xargs)
```

#### I installed a tool, but it's not recognized

You might have to restart the terminal to access the tool. If you have done that and it still does not work then you may have to add it to the PATH. When installing the tool it will often have a command that prints to add it to the PATH.

If that does not work, then restart your system.

#### Incorrect Tool Version

If you have an incorrect version of a required tool, it might cause issues when building and developing smart contracts. To fix this go [here](/broken/pages/3a7bb5afc156c2855c38aa783dfd8161c19fd6ee) to upgrade the version of the tool.

#### Error: script failed: Module\_AlreadyInstalled()

When deploying some smart contracts, if they have a ERC20 Token linked to them then you may have to change the name of the token or destroy the previously deployed contract.
{% endtab %}

{% tab title="MacOS" %}
Common issues and solutions for **MacOS**.

#### Error: Could Not Find World Address

When deploying smart contracts make sure that you are using the correct world address. This is set in the .env for the contracts.

For local worlds the World Address should be:

```bash
0x0165878a594ca255338adfa4d48449f69242eb8f
```

And for Stillness the World Address should be:

```bash
0xcdb380e0cd3949caf70c45c67079f2e27a77fc47
```

The deploy command should look something like this:

```bash
pnpm deploy
```

#### Incorrect Tool Version

If you have an incorrect version of a required tool, it might cause issues when building and developing smart contracts. To fix this go [here](/broken/pages/3a7bb5afc156c2855c38aa783dfd8161c19fd6ee) to upgrade the version of the tool.

#### Error: script failed: Module\_AlreadyInstalled()

When deploying some smart contracts, if they have a ERC20 Token linked to them then you may have to change the name of the token or destroy the previously deployed contract.
{% endtab %}

{% tab title="Linux" %}
Common issues and solutions for **Linux**.

#### Error: Could Not Find World Address

When deploying smart contracts make sure that you are using the correct world address. This is set in the .env for the contracts.

For local worlds the World Address should be:

```bash
0x0165878a594ca255338adfa4d48449f69242eb8f
```

And for Stillness the World Address should be:

```bash
0xcdb380e0cd3949caf70c45c67079f2e27a77fc47
```

The deploy command should look something like this:

```bash
pnpm deploy
```

#### I installed a tool, but it's not recognized

You might have to restart the terminal to access the tool. If you have done that and it still does not work then you may have to add it to the PATH. When installing the tool it will often have a command that prints to add it to the PATH.

If that does not work, then restart your system.

#### Incorrect Tool Version

If you have an incorrect version of a required tool, it might cause issues when building and developing smart contracts. To fix this go [here](/broken/pages/3a7bb5afc156c2855c38aa783dfd8161c19fd6ee) to upgrade the version of the tool.

#### Error: script failed: Module\_AlreadyInstalled()

When deploying some smart contracts, if they have a ERC20 Token linked to them then you may have to change the name of the token or destroy the previously deployed contract.
{% endtab %}
{% endtabs %}

<details>

<summary>Still Having Issues?</summary>

If you're still having issues then you can visit the [EVE Frontier Discord](https://www.discord.gg/evefrontier) to get support!

</details>
