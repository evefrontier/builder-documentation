# Wallet

This page covers some of the most common issues with the Wallet in EVE Frontier and the potential solutions to them.

## Wallet not imported

If you are getting error messages saying you have not imported your wallet, follow the below steps depending on your situation.

### Started on a different device

If you started playing on a different device, you will need to import the recovery phrase from that device. You can do this by either logging into the game and opening the wallet or finding the recovery phrase file.

#### Opening Wallet

{% stepper %}
{% step %}
#### Login to the game on the original device

You need to login and load the game on your original device first.
{% endstep %}

{% step %}
#### Open Your Wallet

Open the wallet through the Keeper Wallet Button:
{% endstep %}

{% step %}
#### Open The Recovery Phrase Window

Click the cog wheel in the EVE section and select the 'View recovery phrase' option
{% endstep %}

{% step %}
#### Copy the Recovery Phrase

This brings up a window with the 12 word recovery phrase for your wallet. Click the copy button to copy the phrase to the clipboard.

{% hint style="warning" %}
Ensure to store this somewhere secure so that you can import it in the future.
{% endhint %}
{% endstep %}

{% step %}
#### Import the phrase

When opening the wallet on the new device you should see a recover wallet button if your wallet is not imported. Click that and then input the recovery phrase from the previous step.
{% endstep %}
{% endstepper %}

#### File

If you want to recover your wallet through the files on your starting device, select the Operating System that you use and follow the below instructions:

{% tabs %}
{% tab title="Windows" %}
For **Windows** you can follow the following steps.

{% stepper %}
{% step %}
#### Navigate To The Folder

Replace **\[USERNAME-HERE]** with your OS username and visit the below folder directory:

{% code title="File path" %}
```
```
{% endcode %}

```bash
C:\Users\[USERNAME-HERE]\AppData\Local\CCP\EVE\c_ccp_eve_frontier_stillness_stillness.servers.evefrontier.com\cache\Wallet
```

Here you'll see folders for all the characters you have created. The folder names are numbers, newest character with the highest number.
{% endstep %}

{% step %}
#### Find .mnemonic\_plaintext

Here you'll see folders for all the characters you have created. The folder names are numbers, with the newest character having the highest number.

In the file explorer, click on the View Button > Show > Hidden Items to be able to view the **.mnemonic\_plaintext** file.

This file contains the recovery phrase which is a 12 word key used to recover your wallet.
{% endstep %}

{% step %}
#### Click the Recover Wallet Button

In-game, when opening the wallet you should see a recover wallet button if your wallet is not imported. Click that and then input the recovery phrase from the previous step.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="MacOS" %}
For **MacOS** you can follow the following steps.

{% stepper %}
{% step %}
#### Navigate To The Folder

You'll need to access the following folder where **\[USERNAME-HERE]** needs to be replaced with your OS username:

{% code title="File path" %}
```
```
{% endcode %}

```bash
/Users/[USERNAME-HERE]/Library/Application Support/CCP/EVE/_users_[USERNAME-HERE]_library_application_support_frontier_sharedcache_stillness_eve.app_contents_resources_build_stillness.servers.evefrontier.com/cache/Wallet
```

_You can access this through Finder by selecting **Go** and **Go to Folder** (shortcut is Command + Shift + G) that brings up a pop-up and there you can paste the folder location above (and replace **\[USERNAME-HERE]** in both places)._
{% endstep %}

{% step %}
#### Find .mnemonic\_plaintext

Here you'll see folders for all the characters you have created. The folder names are numbers, with the newest character having the highest number.

In Finder, press **Command+Shift+Dot** and hidden files will become visible and you should see a file called **.mnemonic\_plaintext**.

This file contains the recovery phrase which is a 12 word key used to recover your wallet.
{% endstep %}

{% step %}
#### Import the phrase

In-game, when opening the wallet you should see a recover wallet button if your wallet is not imported. Click that and then input the recovery phrase from the previous step.
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

### Deleted your cache or reset your device recently

If you recently deleted your cache or reset your device you may have lost your locally stored recovery phrase.

If you saved your 12 word recovery phrase, you can use this to recover it. However, if you didn't save it then you will need to delete your current character and create a new one.

{% hint style="warning" %}
If you recreate your character, make sure to store your assets and items with your Tribe or someone you trust.
{% endhint %}

## Still Having Issues?

If you're still having issues then you can create a ticket at [EVE Frontier Support](https://support.evefrontier.com/hc/en-us/requests/new) to get support.
