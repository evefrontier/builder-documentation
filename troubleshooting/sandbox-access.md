# How to access the Utopia Sandbox Server
{% hint style="info" %}
Note that this server is only available during specific events and is not generally accessible.
{% endhint %}

#### Download the Launcher

* Visit https://evefrontier.com/en/download and select either the Windows or Mac launcher
* Download and install

#### Add the Utopia Server to the launcher
{% tabs %}
{% tab title="Windows" %}
##### Modify an application shortcut
* Right-click on the desktop shortcut to the EVE Frontier Launcher
* Choose Properties
* On the "shortcut" tab add the following to the end of the Target field: ` --frontier-test-servers=Utopia`
* Click "Okay"
{% endtab %}

{% tab title="Mac OS" %}
#### Run the launcher from Terminal
* Open Terminal in the folder where the application resides
* Run the following command: cd /Applications; open 'EVE Frontier.app/' --args --frontier-test-servers=Utopia
{% endtab %}
{% endtabs %}

#### Run the Launcher and Select "Utopia"
You should then see Utopia on the drop down list of servers in the bottom right of the launcher
* Choose Utopia
* Click Register
* Fill in your details
* You will see a prompt to enter a verification code which you will receive by email
* You can then download the Utopia Client
{% hint style="info" %}
After this point you will need to wait for your access to be granted by CCP, until then you will see a "Founder Access Required" Error
{% endhint %}

### Slash Commands
To speed up moving around the universe and gathering materials we have enabled a number of commands you can use on the sandbox
To use these commands simply enter them in the chat window
* `/moveme` This will display a list of star systems which you can move to instantly
* `/giveitem <itemid> <quantity>` This will allow you to spawn the specified item in your current ships' cargo hold
* `/giveitem "<item name>" <quantity>` This works the same way as the above but lets you specify an item by name rather than ItemID
{% hint style="info" %}
Note it is possible to overload your ships cargo this way which will prevent warping, you would first need to move the excess to a storage unit or jettison it
{% endhint %}

Some common items you may want to spawn include the below, more ItemID's can be found via the WorldAPI:

| Name | ItemID |
|------|--------|
| Carbon Weave | 84210 |
| Thermal Composites | 88561 |
| Printed Circuits | 84180 |
| Reinforced Alloys | 84182 |
| Feldspar Crystals | 77800 |
| Hydrated Sulfide Matrix | 77811 |
| Building Foam | 77800 | 89089 |
