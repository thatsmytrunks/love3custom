# LOVE 3: Custom Level Documentation

Hello and welcome to the [LOVE 3](https://love3game.carrd.co). Custom Level Documentation. While you will hopefully find everything you need here to make the level of your dreams, you can always [come join the Discord](https://discord.gg/WvuWxrP3xx) and talk to others about what you need.

If you're looking to upload or download custom levels, you can find them hosted at [mod.io/g/love3](https://mod.io/g/love3)

## Site Map
- [config.json](configjson.md)
- [Editor Mode](editormode.md)
- [Object List](objectlist.md)
- [Object Sets](objectsets.md)
- [Object Variables](objectvariables.md)
- [Sprite Variables](spriteimport.md)
- [Every Sprite in the Game](everysprite.md)
- [Full Sprite / Object Lists](lists.md)

## The Basics

In your install folder is a folder called `custom`. If there are any custom levels in there with properly validated [`config.json`](configjson.md) files, you'll see the **LOVE Custom** option on the main menu.

You'll be using notepad or a text editor to make levels, as well as your image editor of choice. My personal favorites are [Notepad++](https://notepad-plus-plus.org/downloads/) for [editing the JSON file](configjson.md), Adobe Photoshop for editing level geometry, and [Aseprite](https://www.aseprite.org/) for making animated sprites.

Currently, creation of custom levels requires you to edit a text file that has the list of objects and their locations in the level. While it's not as friendly as something like a Super Mario Maker, I think the possibilities available to a level creator are pretty powerful.

If you are looking to create levels, I recommend enabling [Editor Mode](editormode.md).

To make your own levels you'll need to create sprites which you'll define in the `levelCustomSprites` section. I'll try and provide more detail on that later. Once you've created your own sprites, you'll need to create objects that use those sprites in `levelObjects`. Check fredTest1's configuration to get a little bit of an idea on how they work.


[Here](objectlist.md)'s where you can find [a list of just about every object](objectlist.md) in the game.

The main objects in the game are the following:
- p_solid is a solid object that the player can walk on
- p_death will kill the player
- p_bouncer will bounce the player, with the variables `exthSpeed` and `extvSpeed` (horizontal and vertical speed)

## Important Note
• If you have an invalid JSON file in any of your folders, it will likely crash the game every time you go to the level select menu. You can validate your JSON file by going to https://jsonlint.com/