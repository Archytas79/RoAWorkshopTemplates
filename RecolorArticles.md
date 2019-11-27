# Recoloring Articles on the Fly

## Workarounds & Limitations

### LIMITATION: The recolor will be monohue
Color blending only lets us recolor an article with a single hue. Shading will be preserved, but be warned that it cannot be shaded multiple colors.

### Image_blend does not work
This means we cannot use the built-in sprite. We instead have to offload the drawing to one of the draw scripts using:
```
draw_sprite_ext(sprite, subimg, x, y, xscale, yscale, rot, colour, alpha );
```
What we are interested in doing is changing `colour`. `colour` here is the color to blend the sprite with. But since we are using this function, we need to make our own `sprite_index` & `image_index` handles. Replace all instances of these built-in variables with custom variable handles, and use them in internal logic. If you need an `image_number` handle, you can set it to any arbitrary sprite using `im_num = sprite_get_number(sprite_get("sprite_name"));`.
IMPORTANT: Make sure `sprite_index = asset_get("empty_sprite");`. If you use a sprite for collision, you can manually set it so via `mask_index = sprite_get("sprite_name");`

### We cannot grab player colors
So, we need to define player RGB values in init instead of colors so we can pull them later. I recommend a 3 dimensional array, just to keep things tidy:

```
color_list[x][y][z] = value; //Where x is the palette number, y is the color slot, and z (0-2) is R,G, & B respectively. 
```

## Converting Sprites to Grayscale
First, you have to convert all sprites which you want to shade this way to grayscale. Easily done in most image editing software by turning saturation as low as possible.

## pre_draw/post_draw script
```
with obj_article2 {
    if player_id == other.id {
        draw_sprite_ext(article_spr,im_dex,x-x_offset,y-y_offset,1,1,0,other.color_slot,1);
    }
}
```
`color_slot` is a variable of color type handled by the player. Can set this to whatever color you want. Using the above player color array, it can be set a current palette's color:
```
//Update.gml or anywhere which runs on start
if state == PS_SPAWN {
  color_slot = make_color_rgb(color_list[get_player_color(player)][y][0],color_list[get_player_color(player)][y][1],color_list[get_player_color(player)][y][2])
}
```
