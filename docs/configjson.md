[Return to Home](README.md)

# config.json
The `config.json` file is the file that tells your level how to build itself. This page will hopefully help you understand what the `config.json` does. If it doesn't, come ask for help in the **[Mokuzai Studio Discord Server](https://discord.gg/WvuWxrP3xx)**.

## Rules of JSON
JSON files have their own set of rules for syntax. [w3schools](https://www.w3schools.com/js/js_json_syntax.asp) has [a pretty good introduction](https://www.w3schools.com/js/js_json_syntax.asp) to them. A missing comma or a non-integer value without a leading 0 will crash the game. If you find yourself trying to figure out why your JSON's no good, my go-to for finding my mistake is usually to head to [jsonlint.com](https://jsonlint.com/), paste in my full JSON text, and figure out where I've gone wrong.

## The Structure of config.json
config.json is a file that lives in your custom level folder. For the rest of this doc I'll be assuming that you've created and remembered the name of your folder, which should be something simple with no spaces. I find something like my initials and my level name works well. For this example, let's go with `Fred Wood` and `Round And Round`, so let's call the level folder `fwRoundAndRound`.

## An example JSON File with notes

### "levelSettings"
Up first is the `levelSettings`, which will define a lot of the level for you. Everything written with **←** here is notes, with a `*` denoting that the variable can be left blank.
```json
{
	"levelSettings": {
		"levelName": "Round and Round",			← This is your level's name.
		"author": "Fred Wood",					← This is your name, which will be drawn in a couple of places.
		"song": "mus/love1/love14.ogg",			← Here we're using a song that came with the game to avoid copyright issues. You can set this to -1 without quotes to make the level silent.
		"songname": "James Bennett - Kismet",	←* If you include a songname, it will write in the pause menu.
		"roomWidth": 570,						← This is how wide your level is.
		"roomHeight": 240,						← This is how tall your level is.
		"playerX": 3,							← This is the x location of where obj_player will be created.
		"playerY": 38,							← This is the y location.
		"goalX": 569,							← This is the x's value of the goal object that will complete the level when touched.
		"goalY": 55,							← This is the Y value of it.
		"levelcolor": "0f639f"					←* If you want to force the color of your level, you can do that by entering a hex color value in a string
	},
```
### "levelCustomSprites"
The best part of making custom levels is giving them your own visual flair. To do that, you'll need to create some sprites! Here we will define the sprites and give them variables with the `tag` variable. If you want to know more about sprite importing, like sprite strips and offsets, check out the [Sprite Variables page](spriteimport.md). There's no hard limit on how many sprites you can import, either!
```json
	"levelCustomSprites": [
		{
			"path": "custom/fwRoundAndRound/solid.png", ← This is the location of the image file of your custom sprite. 
			"frames": 1,	← This is how many frames of animation the file has. Animations are laid out horizontally in a strip format. 
			"originX": 0,	← The x offset of the sprite.
			"originY": 0,	← The y offset of the sprite
			"tag": "cspr_solid",	← The name we're giving this sprite that we'll reference when giving it to an object.
			"removeback": 1,	← * If your image has a solid color background, you can set this to 1 to remove the background color, defined by the lower left pixel of the image.
			"bbox": {
				"sepmasks": true,		←  Honestly, it's best not to mess with these settings. See the sprite page for detailed information.
				"bbmode": "automatic",
				"bbleft": 0,
				"bbtop": 1,
				"bbright": 1,
				"bbbottom": 1,
				"bbkind": "precise",
				"bbtolerance": 0
			}
		},
		{
			"path": "custom/fwRoundAndRound/death.png", ← Same as above. Make sure you've got / instead of \ for the file path.
			"frames": 1,
			"originX": 0,
			"originY": 0,
			"tag": "cspr_death", ← Make sure not to use the same tag as a previous sprite.
			"bbox": {
				"sepmasks": true,
				"bbmode": "automatic",
				"bbleft": 0,
				"bbtop": 1,
				"bbright": 1,
				"bbbottom": 1,
				"bbkind": "precise",
				"bbtolerance": 0
			}
		}
	],
```
### "levelObjects"
The `levelObjects` section is the real meat and potatoes of the `config.json`. This is where you tell the level to create each object you want, relative to their `originX` and `originY`. Don't forget that you can find a list of just about every object in the game [right here](objectlist.md). Objects can all have their own variables, and lots of them. Read more about [Object Variables here](objectvariables.md).
```json	
	"levelObjects": [
		
			{
				"type": "p_solid",	← Type refers to the object index. Here we're using p_solid, which is the object that denotes safe, solid land.
				"x": 0, ← Because our solid is the size of the stage, we want it at 0,0.
				"y": 0,
				"sprite_index": "cspr_solid", ←* Here's that custom sprite we defined above!
				"image_speed": 0.25,	←* Well, this object isn't animated, but this is where we would put the image speed.
				"scaleX": 1, ←* You can also stretch images! This will set the image_xscale.
				"scaleY": 1,
				"angle": 0	←* You can also rotate images! They look bad when not at an angle divisble by 90, but you do you.
			},
			{
				"type": "obj_lv20_smasher", ← This is an object used in kuso Level 20, and looks great. Let's use it!
				"x": 61,	← We wanna create it at 61,-1, so we set that here.
				"y": -1
			},
			{
				"type":"p_water",	← Water is an invisible object that the player can jump in infinitely.
				"x": 441,	← Here's where the top left of the water starts
				"y": 25,
				"sprite_index":"spr_px1_white",	← p_water doesn't have a visible sprite, so we use this as a placeholder
				"image_xscale":16,	← We stretch the sprite 16 times wide and 160 times tall to cover the water area we want.
				"image_yscale":160,
				"image_alpha":0		← We want this to be invisible, so we set the sprite's alpha to zero.
			},
			{
				"type":"obj_debug_waterdraw_horizontal",
				"x": 441,
				"y": 25,
				"sprite_index":"spr_px1_white",
				"image_xscale":16	
			},
			{
				"type": "obj_orb", ← A cool death object that draws itself at random image_angles flips it randomly. A child of p_death.
				"x": 450,
				"y": 121,
				"depth" : -1	← by default, things are set to a depth of zero. Setting the depth here to -1 will make sure it draws over the p_solid created above.
			},
			{
				"type": "obj_upBouncer", ← This is a child of p_bouncer, and will bounce the player.
				"x": 519,
				"y": 233,
				"extvSpeed":-5 ← We want this bouncer to be extra strong, so we give it an extvSpeed of -5. They're usually -3.
			},
			{
				"type": "obj_ship_creator", ← This will create a helipod carrier here every time one is destroyed.
				"x": 521,
				"y": 139
			},
			{
				"type": "p_death", ← This is a parent object that denotes a level hazard.
				"x": 527,
				"y": 9,
				"sprite_index": "cspr_death" ← And here we're giving it that custom sprite from earlier!
			}
			
	]
}
```

## Closing
Here we are, the end of this explanation. I'm sure there's things that could have been better explained, so if you have questions, make sure to join us in the **[Mokuzai Studio Discord Server](https://discord.gg/WvuWxrP3xx)** where you can ask questions, suggest changes, and request features. That's what the `#love3-custom` channel is for!