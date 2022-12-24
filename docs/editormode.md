[Return to Home](README.md)

# Editor Mode
## About Editor Mode
Editor Mode is a function exclusive to LOVE 3 in LOVE Custom mode. To toggle  Editor Mode, go to the LOVE Custom option on the Main Menu, and press **Ctrl+Shift+E**. This will show an onscreen display noting that it is enabled or disabled. It will save to your save file so you don't have to do it every time you reboot the game.

## Features
- **Right Click**: See the x/y coordinate of your cursor.
- **Left Click**: This will place `obj_player` at the X/Y of your cursor.
- **Ctrl+Shift+F**: Toggle Full Level Display. This will resize your window to a 1:1 scale of the stage.
- **Hold G**: Record a GIF that is dumped in your save folder (usually found in `C:\Users\<username>\AppData\Local\LOVE3`)
- **Ctrl + R** Reset the level and reload the sprites and config.json. This will leave the player in the same spot they're currently standing. I like using this to quickly reload an image file over and over again so you can get the precise jumps down exactly right.
- **Ctrl + Shift + R** Reset the level as above, but reset the player's x/y coordinates to the start of the level.
- **Ctrl + I** to create an object by name at your cursor's x/y coordinates
- **Shift + S** to change the sprite of your cursor to help you preview (does not work with custom sprites)
- **Middle Click/Mouse 3** to toggle debug draw, which will draw the name of the object and the x/y position of it at the bottom of the screen.
- Starting the game with Editor Mode enabled will skip all startup screens and dump you right on to the LOVE Custom level select menu.


## GML Code for Editor Mode
For folks familiar with Game Maker Studio, I'm including the GML for you, in case I missed anything:

The object name is: **obj_lovecustom_editormode**

### Create Event
```GML
	if !global.editormode instance_destroy()
	gif_recording=0
	fullscreen=0
	cursorsprite=spr_px1_white

	drawdebug=false
	image_alpha=0
	sprite_index=spr_px1_white
	depth=-999
```
### Begin Step Event
```GML
	if keyboard_check_pressed(ord("R"))
	&& keyboard_check(vk_control)
	{
		if !keyboard_check(vk_shift)
		{
			if i_ex(obj_player)
			{
				global.editmodeplayerx=obj_player.x
				global.editmodeplayery=obj_player.y
			}
		}
		global.yolo=false
		global.speedrun=false
		scr_newgame("single",room_lovecustom)
	}
```
### Draw Event
```GML
	var xx=window_views_mouse_get_x()
	var yy=window_views_mouse_get_y()
	draw_sprite_ext(cursorsprite,0,xx,yy,1,1,0,c_white,.65)

	if mouse_check_button_pressed(mb_middle)
	{
		drawdebug=1-drawdebug
	}

	if drawdebug
	{
		var _list = ds_list_create();
		var _num = collision_rectangle_list(xx-1,yy-1,xx+1,yy+1,all,true,true, _list, false);

		var cx=camerax(0)
		var cy=cameray(0)+136-3
		
		if _num > 0
		{
			draw_set_color(c_black)
			draw_set_alpha(.95)
			draw_rectangle(cx,cy,cx+240+4,cy-11*_num,false)
			
			draw_set_alpha(1)
			draw_set_color(c_white)
			
			for (var i = 0; i < _num; ++i;)
			{
				//instance_destroy(_list[| i]);
				draw_set_halign(fa_left)
				var write=	object_get_name(_list[| i].object_index) + "  |  "
							+"X:"+string((_list[| i].x)) + "  |  Y:"+string((_list[| i].y))
				scr_draw_text(cx,cy-_num*11-1+i*11,write)
			}
		}
		ds_list_destroy(_list);
		
		
	}

	if mouse_check_button(mb_right)
	{
		scr_draw_text(xx+2,yy+2,"x="+string(xx))
		scr_draw_text(xx+2,yy+12,"y="+string(yy))
	}
```

### Draw GUI Event
```GML
	var xx=window_views_mouse_get_x()
	var yy=window_views_mouse_get_y()

	if mouse_check_button_pressed(mb_left) 
	&& i_ex(obj_player)
	{
		obj_player.x=xx
		obj_player.y=yy
		obj_player.vsp=0
		obj_player.falltime=0
	}


	//GIF Recorder
	{
	if keyboard_check_pressed(ord("G")) and gif_recording=0
	   {
		gif_recording=1
		gif_timer=0
		gif_date=
			string(date_get_year(date_current_datetime()))+"_"+
			string(date_get_month(date_current_datetime()))+"_"+
			string(date_get_day(date_current_datetime()))+"_"+
			string(date_get_hour(date_current_datetime()))+"_"+
			string(date_get_minute(date_current_datetime()))+"_"+
			string(date_get_second(date_current_datetime()))
	   }
	   
		if gif_recording
		{
			var gif_release=0
		
			if keyboard_check_released(ord("G")) gif_release=1
		
			if gif_timer == 0
				{
					gif_image = gif_open(camera_get_view_width(0),camera_get_view_height(0));
				}
		
			else if gif_timer < 1200 and gif_release=0
				{
					gif_add_surface(gif_image, application_surface, 2);	// Since this is taking from the application surface, it won't properly apply the color swap shader by James Begg
				}
		
			else
				{
					gif_save(gif_image,  "love3screencap_"+gif_date+".gif");	//this will dump to your Save Directory
					gif_timer = 0;
					gif_recording = false;
				}
			gif_timer++;
	}
	}


	if keyboard_check(vk_control)
	&& keyboard_check_pressed(ord("F"))
	{
		if fullscreen=1
		{
			fullscreen=0
			//with obj_player instance_destroy()
			_master.camera=true
			window_set_size(240*global.vidMode,136*global.vidMode)
			camera_set_view_pos(view_camera[0],0,0)
			camera_set_view_size(view_camera[0],240,136);
			view_set_wport(view_camera[0],240)
			view_set_hport(view_camera[0],136)
			surface_resize(application_surface,240,136)
			_master.alarm[1]=1
		}
		else
		{
			fullscreen=1
			//with obj_player instance_destroy()
			_master.camera=false
			window_set_size(room_width*1,room_height*1)
			camera_set_view_pos(view_camera[0],0,0)
			camera_set_view_size(view_camera[0],room_width,room_height);
			view_set_wport(view_camera[0],room_width)
			view_set_hport(view_camera[0],room_height)
			surface_resize(application_surface,room_width,room_height)
			_master.alarm[1]=1
		}
	}

	if keyboard_check(vk_shift)
	&& keyboard_check_pressed(ord("S"))
	{
		var newsprite=get_string("Enter sprite name: (current="+ sprite_get_name(cursorsprite) +")", "")
		var newspriteindex=asset_get_index(newsprite)
		if sprite_exists(newspriteindex) cursorsprite=newspriteindex
		else						cursorsprite=spr_px1_white
	}


	if (keyboard_check(vk_shift) || keyboard_check(vk_control))
	&& keyboard_check_pressed(ord("I"))
	{
		var instname=get_string("Enter Object Name: (ex: obj_marker)","")
		if object_exists(asset_get_index(instname))
		{
			instance_create(xx,yy,asset_get_index(instname))
		}
	}
```
