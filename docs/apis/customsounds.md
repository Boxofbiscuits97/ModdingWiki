---
prev: false
next: false
description: A guide to replacing sounds in Lethal Company with custom versions using the CustomSounds mod. 
---

# A guide to Lethal Company audio modding (using CustomSounds)

::: warning 
This is not a tutorial on how to replace audio with BepInEx using Harmony Patching or with LCSoundTool. This is a guide for using CustomSounds to replace audio. If you want to make a standalone audio mod without CustomSounds, this guide is not for you.
:::

## Installing the mods
You need to install the following mods to be able to add custom sounds:

- [BepinExPack by BepInEx](https://thunderstore.io/c/lethal-company/p/BepInEx/BepInExPack/)

- [LCSoundTool by no00ob](https://thunderstore.io/c/lethal-company/p/no00ob/LCSoundTool/)

- [CustomSounds by Clementinise](https://thunderstore.io/c/lethal-company/p/Clementinise/CustomSounds/)

If you're using r2modman or the Thunderstore manager, your basic setup should look like this:

![Image of r2modman setup](/images/customsounds/r2modman_setup.png)

## Finding the audio files 
If you want to replace an audio file, you need to know which sound you're actually replacing. For the sake of the guide, I'm going to try and find the audio files for the "Icecream Truck" music whenever the delivery ship lands, but you can apply this process to any sound you'd like to replace.

### Extracting the audio files
To find the sound file you want to replace, I personally recommend extracting all the audio files from the game so you have easy access to them, which makes finding them a lot easier. 

You can do this by using a program called [AssetRipper](https://github.com/AssetRipper/AssetRipper/releases). Make sure to use the latest version (NOT the pre-release one).

First, open up AssetRipper and select the Audio Export Format as "Convert to WAV", as the audio mods we're using can only handle .wav files. 

Afterwards, on the top toolbar go to `File > Open Folder`, and select "Lethal Company_Data", which you can find in the directory where Lethal Company is installed (if you don't know where your game is installed, just go to Steam, right click on Lethal Company, and then go to `Manage > Browse Local Files`).

If everything worked correctly, you should be seeing this window:

![Image of Assetripper](/images/customsounds/AssetRipper.png)

Now, to extract ALL the audio files from the game, you need to find an AudioClip file in one of the sharedassets folders. 

For example, if you click on the arrow next to `sharedassets0` and then the one next to AudioClip you should be able to see the files `BootUp`, `ButtonPress1` and `HoverButton1`. Now, click on any one of these files so that it's selected, and at the top click on `Export > Export All Files of Selected Type`. 

Afterwards, select where you would like to save the audio files and click Select Folder. At the time of writing this guide, there should be exactly 769 audio files that you can export.

Afterwards, go to the directory you exported the audio files to, and go to `Lethal Company\ExportedProject\Assets\AudioClip`, and there, you should be able to see all the audio files that are currently in the game! Feel free to copy these files somewhere else or to delete the `.meta` files.

You can usually find the files just by looking at the audio names, they have pretty logical names. For example, the ship "ice cream truck music" is literally called `"IcecreamTruckV2"` and `"IcecreamTruckFar"`. 

However, if you REALLY can't find the sound file you're looking for, then [**Step 2.2**](#using-lcsoundtool) is for you. Otherwise, you can go directly to [**Step 3**](#replacing-the-audio-files).

### Using LCSoundTool

::: tip
If you already have found the audio file you're looking for, you can skip directly to the next section.
:::

If you can't find the sound you're replacing by just looking at the extracted audio files, then you can use method this to find out the exact name of the sound file.

LCSoundTool has a logging function which can be activated / deactivated by pressing F5 when in the game, and it prints the current sound being played to the BepinEx console. For this to actually show up in the console, you need to edit the BepInEx.cfg file, which you can access by going to BepInEx/config/BepInEx.cfg, or if you're using r2modman / Thunderstore, by going into Config Editor and BePinEx.cfg as shown in this image:

![Image of config in r2modman](/images/customsounds/r2modman_config.png)

In particular, you need to change 4 settings to the following parameters:
```
[Chainloader] HideManagerGameObject = true
[Logging.Console] Enabled = true
[Logging.Console] LogLevels = Fatal, Error, Warning, Message, Info, Debug
[Logging.Disk] WriteUnityLog = false
```
Make sure to change the LogLevels in Logging.Console, I've seen a lot of people skipping this step because they thought everything was included when they missed adding Debug. Once you're done editing the cfg file, press Save at the top of r2modman / Thunderstore or just save normally if you're accessing the file directly.

Now when you boot up the game you should see the following message whenever you press F5 to enable / disable the logging:
```
[Debug  :LCSoundTool] Toggling AudioSource debug logs True!
```
If you want to find out the sounds of enemies, I recommend downloading the mod "**GameMaster**" as it allows you to spawn enemies and give yourself god mode so you don't die while you look at the console, and a bunch of other options that make this process easier.

Anyways, for the "Ice cream truck" music, I ordered an item through the terminal and spawned into a map, and once it arrives you can clearly see which sounds are responsible for playing the music:

![Image of BepInEx Console](/images/customsounds/BepInEx_console.png)

So now we know that we need to replace the sound files `IcecreamTruckFar` and `IcecreamTruckV2` to make it play custom music! You can repeat this process for (almost) all sounds, so if you want to know what sounds the Jester enemy makes, then you spawn in a Jester enemy and wait until it plays the sound effect you want to replace.

## Replacing the audio files
So, now we know that the files we're looking for are `IcecreamTruckFar` and `IcecreamTruckV2`. How do you replace them?

First, you need to rename whatever audio file you're going to use to replace the original one, and (if it isn't already) change the audio format to `.wav`. So if you have a file called `IcecreamMusic.mp3`, then you need to rename it to `IcecreamTruckV2` and convert it to `.wav`.

Once you're done with that, you need to place the audio files in the folder where CustomSounds is installed.

For the **manual installers**:

 Go to `BepInEx\plugins\CustomSounds` and place the files either directly in there or make another folder inside of CustomSounds and put them there.

For **r2modman \ Thunderstore** users:

Go to settings>Browse profile folder, and then go into `BepInEx\plugins\Clementinise-CustomSounds\CustomSounds` and either place the audio files directly there or make another folder inside of CustomSounds and put them there.

**Note:** r2modman WILL delete your custom sound files each time you update CustomSounds, so I would recommend to keep your replacement sounds in another folder so you can quickly swap them back in the case an update comes out.

### Additional features

CustomSounds has added a few features with Update 2.1.0. This is a rewrite of the CHANGELOG taken from Thunderstore:

You can now use multiple files to replace one sound effect, and then set a random % chance for each of these replacement sounds to play. So, for example, you can replace the JackOLanternHit sound with two sound files, change their names into JackOLanternHit-25.wav and JackOLanternHit-75.wav, and they will respectively have a 25% and a 75% chance of playing that particular sound.

Additionally, you can now add short descriptions to sounds that show up in the terminal when you type in "customsounds list". For instance, if you want to add a description to the JackOLanternHit.wav sound file, you would just name it JackOLanternHit-Funny-Laugh.wav, and it will display as "JackOLanternHit [Funny Laugh]" in the terminal.

## Testing the replacement
If you did everything correctly, you should see the files loading in the BepInEx console. Alternatively, you can just go into your ship and type "customsounds help" in your terminal to see all the available commands. If your replacement sounds have loaded correctly, then you should be able to see them if you type in "customsounds list" like this:

![Image of terminal using the CustomSounds command](/images/customsounds/CustomSounds_console.png)

Here's a little fun demo of how this particular setup looks like:

[![Watch the video](/images/customsounds/Youtube_thumbnail.png)](https://youtu.be/mk8O8qFcMlk)

## Sharing custom sounds with friends

### File sharing

This one is by far the most obvious and simple method, you just put the audio files into a `.zip` file, upload it somewhere, tell your friends to download it and put it into the CustomSounds folder, and that's it.

### Customsounds syncing feature

The second one uses a new experimental feature of CustomSounds, which allows your friends to synchronize their custom sounds with yours. As before, you can type in "customsounds help" to see all the available commands. The command of interest to you is the "customsounds sync" command. Once you type in this command, everyone in your lobby should get this prompt:

![Image of Sync request](/images/customsounds/Sync_request_client.png)

Once the clients press F8, the host will send all the custom sounds to all clients, and they will be immediately loaded by the clients once the download is complete. In the case that someone doesn't want to use your custom sounds, they can use "customsounds revert" to use the original game files instead.

### Uploading to Thunderstore

The only thing you need to keep in mind when publishing your mod is to keep this particular folder structure:

```
- manifest.json
- README.md
- CHANGELOG.md (Optional)
- icon.png
- BepInEx
    - plugins
        - CustomSounds
            - <YourSoundPackName>
                - [Insert All Audio Files Here]
```

For the rest, check out the section on how to [publish your mod](/modding/publishing-your-mod) here on this wiki. 

## Credits

If you have any questions then feel free to ping me over on the [Lethal Company Discord](https://discord.gg/lethal-company), or just DM me on Discord (nickname: `futuresavior`). Please do NOT send me friend requests on Discord, just DM me directly, and don't forget to have fun \:)

Thank you to Clementinise for creating CustomSounds and proof-reading my guide, and to no00ob for creating LCSoundTool, both of your mods are amazing and appreciated \<3
