# Article Particle Template

### By Archytas

## Description
This template includes code for making particles with logic using an article, including code for:

- [x] Reading sprites

- [x] Animating sprites
 
- [x] Debug info

# Scripts

## article#_init.gml

#### Code
```
//article3_init

sprite_index = sprite_get("your_sprite_here");

//Article Vars
can_be_grounded = false;
free = true;
uses_shader = true;

//Custom Vars
init = 0;
state_timer = 0;
anim_speed = 2;
build_time = 100;
reverse = 0;
index_override = 0;
```

#### Details
Variable             | Description
-------------------- | -----------
`state_timer`     | How long the article instance has been alive, in frames
`reverse`    | If the sprite reverses it's animation before deletion
`index_override`    | If you want to control the sprite manually (it will keep a single sprite for build_time)
`anim_speed`    | How long will the sprite be alive. (coarse control)
`build_time`    | How long will the sprite be alive. (if index_override is 1, this will be the frame count) (fine control)
`"your_sprite_here"` | The name of the sprite file, of the form your_sprite_here_strip#.png where # is the number of frames.
## article#_update.gml

Replace '#' with whatever article number you want to use as the particle.

#### Code
```
//article#_update

if (state == 0) { //Animating
    if index_override == 0 {
        image_index = ((state_timer*anim_speed)/build_time)*(image_number);
        if (image_index == image_number) {
            if reverse == 0 {
                instance_destroy();
                exit;
            } else state = 2;
        }
    } else if index_override == 1 {
		if (state_timer == build_time) {
            if reverse == 0 {
                instance_destroy();
                exit;
            } else state = 2;
        }
	}
    state_timer++;
    //Custom Code Here
	
    //
    
}

if (state == 2) {
    if index_override == 0 {
        image_index = ((state_timer*anim_speed)/build_time)*(image_number);
        state_timer--;
        if (state_timer*anim_speed == 0) {
            instance_destroy();
            exit;
        }
    } else {
        instance_destroy();
        exit;
    }
}

if (init == 0) {
    init = 1;
	state = 0;
    //first-spawn events here
	
	//
}
```

#### Details

This is the raw article code. You can put any custom code inside the state == 0 statement. 
Air friction, other effects, hitboxes/hurtboxes, etc. The sky's the limit!

## Article Spawn Code

You can put this code in any gml file. One stipulation:
 - Make sure the namespace is the custom character, and not an article or opponent.


#### Code
```
// Particle Spawn Code
var par_vfx = instance_create(x,y,"obj_article#");
with par_vfx {
	//Custom initialization values here
	spr_dir = other.spr_dir
	sprite_index = sprite_get("your_other_sprite");
	//
    }
```

#### Details

This code spawns the particle and allows for you to add custom initialization values to the particle depending on how it was spawned.
Can override anything in the article initialization file with values in the with statement. Remember to use the *other* keyword to reference player variables when inside the with statement.
- NOTE: par_vfx is a temporary handle that only refers to the newest particle. Put code that needs to run each frame inside the article update file, or replace par_vfx with a more permanent handle.