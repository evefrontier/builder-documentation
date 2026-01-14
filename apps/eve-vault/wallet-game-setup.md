# Wallet Game Setup

## Add your Wallet to the EVE Vault in game

When you have created a character we automatically create a wallet that you need to connect to the EVE Vault to see it in game.

{% stepper %}
{% step %}
### Open the Wallet

Open the **Wallet** in-game from the lower left Keeper wallet button:
{% endstep %}

{% step %}
### Open the EVE Vault

Open the **EVE Vault** in the wallet by clicking the orange **EVE** Tab:
{% endstep %}

{% step %}
### View recovery phrase

Click the cog wheel on the EVE Tab in the _wallet window_ and select the "View recovery phrase" option:

{% hint style="warning" %}
**IMPORTANT:** Do not lose your recovery phrase. Without this phrase you could permanently lose access to your wallet and your digital assets. We recommend storing it somewhere safe—for example in a secure physical location (by writing it down)—and not sharing it with others.

If you log into your account on another computer you must have your original recovery phrase to recover your digital assets on another computer.
{% endhint %}
{% endstep %}

{% step %}
### Copy the 12-word phrase

This brings up a window with the 12 word recovery phrase for your wallet. Click the "Click to copy" button to copy it.
{% endstep %}

{% step %}
### Import character wallet

Now click the "Import character wallet" button in the EVE Vault:
{% endstep %}

{% step %}
### Paste and confirm

Paste or type your recovery phrase into the text area and click **Confirm**.
{% endstep %}

{% step %}
### Set EVE Vault password

You'll then be asked for a password for locking the EVE Vault (the wallet you just added). Select a password (and don't forget it or keep it safe somewhere).
{% endstep %}

{% step %}
### Wallets visible

Now you should see **EVE** and **GAS** in your wallet in the EVE Vault.
{% endstep %}
{% endstepper %}

<details>

<summary>What happens when you delete your character?</summary>

When you delete a character, the EVE Vault and any wallets stored in it remain accessible and are not removed or cleared.

If you create a new character and open the EVE Vault, you will be prompted to enter the password to access it, just as before. The inventory, including your game wallet, will still be intact.

_For the current playtest, players are given a single starting wallet for their characters. However, this will be updated later so that you can manually add and remove wallets as you wish._

</details>

<details>

<summary>Why is there an in-game password for the EVE Vault?</summary>

The password protects access to the EVE Vault inventory within the game, as it contains items stored on the blockchain. _As soon as you add a wallet to the EVE Vault you are asked to protect it with a password to protect the inventory inside._

Anything in the EVE Vault is not tied to the game client but resides on the blockchain, making it accessible from other platforms, not just the in-game EVE Vault.

_For example, you can access the EVE Vault at_ [_vault.evefrontier.com_](https://vault.evefrontier.com/)_, where you also need to set up a password for the browser. This password can be different from the one you use for the in-game EVE Vault._

</details>

<details>

<summary>How is the recovery phrase stored?</summary>

The recovery phrase is currently stored in plaintext on the computer where your character was created. Since the playtest is focused on testing wallet functionality, we are not addressing this immediately.

{% hint style="info" %}
_This will be replaced with Account Abstraction in future updates._
{% endhint %}

</details>
