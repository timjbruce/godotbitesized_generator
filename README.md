# Godot Bite Sized - Generator

I really like games that have random map/room generation capabilities. Probably doesn't surprise you that I like to build games that use one. This repo contains the Generator object we'll be building up to generate rooms and floors/levels.

## Build 1
**Build 1 is in Godot 4.5.1**

For this build, we're building a single node object that will be used by Worlds to procedurally create, eventually, a floor/level for a 2D game. The first version is going to create a single room. This will also be the first object that starts to expose variables that can be used by the World to change the size of the object. When you get done with this build, your Scene and FileSystem windows should look similar to this.

![Your final scene and filesystem should resemble this output](readmeassets/build_1/1_final_scene_and_filesystem.png)

#### Starting the Scene

Let's start with a new project in Godot. In the Scene window, select "Node 2D." This is going to be the base node for our Generator. 

![Create the root node for the scene as a Node 2D node](readmeassets/build_1/1_create_generator_node.png)

Rename the base node as "Generator."

![Rename the base node for the generator to Generator](readmeassets/build_1/1_rename_base_node.png)

Next we'll add a Control Node to the scene. Click the + and search for the "Control" node. Control nodes are the base UI classes in Godot and give us a lot of flexibility for building, in this case, a dynamically sized room. Click the "Create" button to move into the scene paint mode.

![Add control node to the scene by clicking + and searching for the contorl node type](readmeassets/build_1/1_add_control_node.png)

Next, let's rename the `Control` node to `GeneratorNode` by double-clicking it and typing in the new name.

![Rename the control node to Generatorengine](/readmeassets/build_1/1_rename_control_to_generator_node.png)

Let's save the `Generator` scene now into the "Generator" folder. When you click save, create a new folder called "Generator" and save the `Generator` scene there as "generator.tscn." Your FileSystem window should look like this.

![Save the Generator scene into a folder named Generator](readmeassets/build_1/1_generator_scene_saved.png)

### Adding a Default Tile Map

In the Generator scene, we're going to add a default TileMapLayer for the Generator to use. This is going to be used for testing and in case one isn't added at the world level.

Click on the "Generator" node in the Scene window and click the + button to open the "Create New Node" dialog window.

![Add a node to the Generator by clicking the + button to create a new node](readmeassets/build_1/1_create_new_node_tilemap.png)

Search for the "TileMapLayer," select it, and click the "Create" button to add it to the scene.

![Add the TileMapLayer node to your Generator scene](readmeassets/build_1/1_search_for_tile_map.png)

And let's rename the TileMapLayer to "DefaultTileMap." Double-click on the TileMapLayer and type the new name as so.

![Renamed defaulttilemap](readmeassets/build_1/1_renamed_defaulttilemap.png)

Now for the part that is going to take a bit, building the default tile map. You're probably going to replace this tilemap with every new game world you build, but this is good for testing updates to your Generator without having to import and setup a new graphic or take this generator and put it into a game world. 

Start by making sure your DefaultTileMap is selected in the Scene window. The next set of steps will be in the Inspector window. Click on the down arrow by the "Tile Set" field.

![Create the tile set by clicking the down arrow next to the TileSet field and selecting the TileSet menu option](readmeassets/build_1/1_creating_the_tileset.png)

Next, click on the words "TileSet" in the Tile Set field. This will open options for the TileSet itself. See below for where to click.

![Open up the details of the TileSet by clicking on the TileSet value in the Tile Set field](readmeassets/build_1/1_select_the_tileset.png)

The first thing we're going to do with the TileSet is to add a Physics layer. Nothing else, right now, in our components or World has a physics layer. We specifically want to add this, as we're going to build walls with this Generator and the Player shouldn't be able to move beyond the walls. Even with this added layer to the Generator, the Player will be able to move beyond the walls, but we can fix that with the next Player build!

Click on the "Physics Layers" section in the TileSet to open it up. Click on "+Add Element." Now, change the "Collision Layer" so that nothing is selected and the "Collision Mask" so that only "2" is selected, as so.

![Set the collision layer and collision mask for the DefaultTileSet](/readmeassets/build_1/1_set_collision_layer.png)

Next, we will add in "Terrain Sets" by clicking that header in the same TileSet. We will then click the "+Add Element" button to open our first terrain set, as so.

![Create the initial terrain set for the DefaultTileSet](/readmeassets/build_1/1_terrain_set_added.png)

Now, click on the "Terrains" sub heading.

![Open the terrain details by clicking on the Terrains down arrow](/readmeassets/build_1/1_open_the_terrain.png)

Now that we have the terrain open, you're going to click "+ Add Eelement" for the Terrain four times. I know there's a lot of "+Add Elements" here - make sure you click the right one!

![Click on +Add Element for the Terrain to add 4 terrains to this set](/readmeassets/build_1/1_click_the_right_add_element.png)

Your Inspector window should now look like this:

![Your four terrains should now be added to this Terrain](/readmeassets/build_1/1_four_terrains_added.png)

Lastly, for the terrain sets, let's rename each of them to "Wall," "Floor," "Filler," and "Exit" from the top down. The result should look like the below.

![Rename the terrains so that the names are wall, floor, filler, and exit](/readmeassets/build_1/1_terrains_renamed.png)

This last bit of the TileSet has a few steps. There is an asset in the repo located at /Generator/assets/tilemap.png. You'll want to copy that or make your own asset to use for this part. When the DefaultTileMap is selected, you should see a painting area that is opened at the bottom of your window, assuming you use the defaults, that looks like this.

![Look for the tileset painting window at the bottom of your Godot dock](/readmeassets/build_1/1_tile_set_painting_window.png)

Click on the "TileSet" at the bottom of that window. That will open a new view for us to use the asset mentioned above.

![Click the TileSet at the bottom of the painting window](/readmeassets/build_1/1_click_on_tileset.png)

Now, select and drag your tilemap.png file to the "Tile Sources" field in the new painting window.

![Drag your tilemap to the Tile Sources field in the TileSet window](/readmeassets/build_1/1_click_and_drag_tileset.png)

You should now see a dialog that your atlas was updated. Go ahead and click "Yes" to auto create the tiles.

![Auto create the tiles to update your atlas](/readmeassets/build_1/1_autocreate_tiles.png)

Now we need to create the Terrain tiles that will be used by the Terrain Layers that we created above. We'll open the setup window first by double-clicking on the tilemap in the Tile Sources field.

![Double-click the tilemap to open the setup window for the tilemap](/readmeassets/build_1/1_double_click_tilemap.png)

Edit the ID value to "0" by clicking the "Edit" button and updating the value in the dialog to "0." Click "OK" when you've done this.

![Change the Source ID value of the TileSource to 0](/readmeassets/build_1/1_edit_source_id.png)

Change the name of the Tile Source to "Default."

![Set the name of the TileSource to Default](/readmeassets/build_1/1_set_tilesource_name_to_default.png)

Now, we're going to click on "Paint" and use that to setup our terrain tiles and physics layer 0.

![Now move to painting the tiles that we'll use in the world](/readmeassets/build_1/1_start_painting_tiles.png)

Under "Paint Properties," select the "Terrains" option.

![Select the Terrains option to Paint first](/readmeassets//build_1/1_select_terrains.png)

Select Terrain Set 0 in the Terrain Set field and Wall in the Terrain field.

![Select the terrain set we'll be updating, Terrain set 0, and the type of terrain, the wall.](/readmeassets/build_1/1_select_terrain_0_and_wall.png)

Click in the middle of the upper left dark brown box in the tile map. A faint brown box should now be selected in it.

![Select the tile for the Wall from the upper left tile in the tilemap, using the middle of the tile](/readmeassets/build_1/1_wall_tile_selected.png)

Now set the Floor, Filler, and Exit terrains the same way, using the middle of the upper left box. The colors should be light brown for Floor, blue for Filler, and black for Exit. The result should look like this.

![Set the other terrains and validate the single tile is selected](/readmeassets/build_1/1_all_terrains_selected.png)

Next, select the Physics Layer 0 value in the Paint Properties field and select the top left brown tile.

![Set the physics layer](/readmeassets/build_1/1_set_physics_layer.png)

Save your scene here. The end is now in sight!

### Adding Code to the Generator and GeneratorNode

Now, we're going to add a script to the `Generator` by selecting the "Generator" node and clicking the "Attach new or existing script to the selected node" button.

![Add a script to the generator node by clicking the add script button](readmeassets/build_1/1_add_script_to_generator.png)

Use the default values in the window and click "Create" to add the script. This will open the code editor.

![Accept the default values to add the generator script to the generator node](readmeassets/build_1/1_accept_default_to_add_script.png)

In the code editor, we're going to place the following code

```
extends Node2D
class_name Generator

@export var min_room_width: int = 3
@export var min_room_height: int = 3
@export var max_room_width: int = 500
@export var max_room_height: int = 500
@export var game_width: int = 1600
@export var game_height: int = 1200
@export var tile_map_layer: TileMapLayer = null
@export var draw_outline: bool = false


signal floor_generated
signal floor_is_cleared


func _ready() -> void:
	$GeneratorNode.initialize(min_room_width, min_room_height, max_room_width, max_room_height, game_width,
		game_height, tile_map_layer, draw_outline)
	if get_parent().name == "root":
		$GeneratorNode.generate()
		floor_generated.emit()


func generate() -> void:
	$GeneratorNode.generate()
	floor_generated.emit()


func clear_floor() -> void:
	$GeneratorNode.clear_floor()
	floor_is_cleared.emit()

```

This code acts as a wrapper for the GeneratorNode, as it is the root object of the scene. It takes in the default values and will set them for the GeneratorNode to use when generating a room. It will also take it a "TileMapLayer" that can be used to define what tiles should be used as "Floor," "Wall," "Filler," and "Exit." When these values are filled in and "generate" is called, the random room will appear in the game world, once we add that code to the GeneratorNode. There is also a signal for "floor_generated," which can be used to signal other tasks that might require the floor to be in place before taking action.

There is also a "clear_floor" function, which is used to clear out the assets for the existing floor, which also sends out the signal that the floor is cleared. This can be used to track the state of the floor for the game world.

These signals are being added in advance. Right now, nothing in our current "World" cares about the state of the floor. In the future, however, you will add in code to generate the player, enemies, objects, loot, and whatever you can come up with. If you have objects that create these types of items, they can subscribe to these signals to take appropriate actions.

Next, we will add script to the GeneratorNode object. Click the GeneratorNode object and click the "Attach new or existing script to the selected node" button.

![Add a script to the generatornode node by clicking the add script button](readmeassets/build_1/1_add_script_to_generatornode.png)

```
extends Control
class_name GeneratorNode

var min_room_width: int = 3
var min_room_height: int = 3
var max_room_width: int = 500
var max_room_height: int = 500
var game_width: int = 1600
var game_height: int = 1200
var tile_map_layer: TileMapLayer = null
var draw_outline: bool = false

var wall_vector: Vector2i
var floor_vector: Vector2i
var filler_vector: Vector2i
var exit_vector: Vector2i

var room_listing: Dictionary


func clear_floor() -> void:
	room_listing = {}
	tile_map_layer.clear()


func generate() -> void:
	var grid: Dictionary = {Vector2i(0,0): {"x_start": 0, "y_start": 0}}
	room_listing = _generate_room(grid)
	_draw_rooms()


func initialize(inc_min_room_width: int, inc_min_room_height: int, inc_max_room_width: int, inc_max_room_height: int,
				inc_game_width: int, inc_game_height: int, inc_tile_map_layer: TileMapLayer, inc_draw_outline: bool) -> void:
	min_room_width = inc_min_room_width
	min_room_height = inc_min_room_height
	max_room_width = inc_max_room_width
	max_room_height = inc_max_room_height
	game_width = inc_game_width
	game_height = inc_game_height
	tile_map_layer = inc_tile_map_layer
	draw_outline = inc_draw_outline
	_get_main_tiles()

func _ready() -> void:
	pass
	
	
func _get_main_tiles() -> void:
	var terrains = {}
	for terrain in tile_map_layer.tile_set.get_terrains_count(0):
		terrains[str(terrain)] = tile_map_layer.tile_set.get_terrain_name(0,terrain)
	var source = tile_map_layer.tile_set.get_source(0)
	#check every tile to find its terrain
	for tile in source.get_tiles_count():
		var tile_data = source.get_tile_data(source.get_tile_id(tile), 0)
		if tile_data.terrain != -1:
			match terrains[str(tile_data.terrain)]:
				"Wall":
					wall_vector = source.get_tile_id(tile)
				"Floor":
					floor_vector = source.get_tile_id(tile)
				"Filler":
					filler_vector = source.get_tile_id(tile)
				"Exit":
					exit_vector = source.get_tile_id(tile)


func _generate_room(grid_location) -> Dictionary:
	var room: Dictionary = {}
	for item in grid_location:
		room["x"] = randi_range(grid_location[item]["x_start"], 
			grid_location[item]["x_start"])
		room["y"] = randi_range(grid_location[item]["y_start"], 
			grid_location[item]["y_start"])
		room["width"] = randi_range(min_room_width, max_room_width - (room["x"] - grid_location[item]["x_start"]))
		room["height"] = randi_range(min_room_height, max_room_height - (room["y"] - grid_location[item]["y_start"]))
	return room


func _draw_rooms() -> void:
	if draw_outline:
		for x in range(0, game_width):
			if x % 25 == 0:
				tile_map_layer.set_cell(Vector2i(x,0), 0, floor_vector)
			else:
				tile_map_layer.set_cell(Vector2i(x,0), 0, filler_vector)
		for y in range(0, game_height):
			if y % 25 == 0:
				tile_map_layer.set_cell(Vector2i(0,y), 0, floor_vector)
			else:
				tile_map_layer.set_cell(Vector2i(0,y), 0, filler_vector)

	for x in range(room_listing["x"], (room_listing["x"] + room_listing["width"] + 1)):
		for y in range(room_listing["y"], (room_listing["y"] + room_listing["height"] + 1)):
			if x == room_listing["x"] || x == (room_listing["x"] + room_listing["width"]):
				tile_map_layer.set_cell(Vector2i(x,y), 0, wall_vector)
			elif y == room_listing["y"] || y == (room_listing["y"] + room_listing["height"]):
				tile_map_layer.set_cell(Vector2i(x,y), 0, wall_vector)

```

This code will generate a random sized room using the min/max_room_width and min/max_room_height variables. There's also a flag to draw the outline for the full game world, should you need it for debugging.

### Almost ready to generate a room!

The last little bit that we need to do is to explore the properties of the Generator and how we can use them in a game. Click on the "Generator" in your scene and look at the Inspector to see what is exposed.

![View the generator properties in the inspector window and see the default values](/readmeassets/build_1/1_generator_properties.png)

click on the Tile Map Layer field in the properties to open up a dialog to select a node to assign to the property. Click on the "DefaultTileMap" node and click "OK" to assign our Default Tile Map.

![Assign the DefaultTileMap to our Generator node so that it can use the tiles to draw the map](/readmeassets/build_1/1_assign_tile_map.png)

### Let's Test

Since this is the only scene, we're going to test using the F6 / Run Scene functionality of Godot. Make sure the Generator Scene is open in the Scene window and press F6. You should see a window something like the below open up. Try out different settings for the Generator in the Inspector window to set different room sizes.

![A room will be generated and you'll see a dark brown line around the room](/readmeassets/build_1/1_room_generated.png)