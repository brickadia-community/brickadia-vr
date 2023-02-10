# Brickadia VR Guide
Instructions for using Unreal Engine to view Brickadia models in Virtual Reality
https://streamable.com/fo2t4k

## Prerequisites
1. Install the Epic Games Launcher if you don't already have it
https://store.epicgames.com/en-US/download
2. Install Unreal Engine (this tutorial is using Unreal Engine 5.1 but version shouldn't matter too much)

## Exporting the Brickadia Save
- Start by loading the save you want to export in Brickadia
- Open up the console using the `~ key (left of 1 key on most keyboards)
- Run the following command to export the save as a gLTF model
```
Bricks.ExportModel ModelName
```
![image](https://user-images.githubusercontent.com/3935931/218205689-b4c17540-1643-4f43-9768-397dd2133b7b.png)

You should now see the file for the exported model in this directory
```yaml
%LocalAppData%\Brickadia\Saved\ExportedModels
```

![image](https://user-images.githubusercontent.com/3935931/218205806-b54ab018-5b27-494f-a258-93af7249d37e.png)


## Creating the Unreal Engine Project
Launch Unreal Engine and select the Virtual Reality Template from the Games tab

<img src="https://user-images.githubusercontent.com/3935931/218202976-baa14916-17d9-4073-850a-dcba7a284917.png" width="500"/>

After hitting create, you should now see the default VR scene in your editor

Set the preview mode to VR

<img src="https://user-images.githubusercontent.com/3935931/218206272-51027e45-bfad-45e7-a468-b74d84e910e4.png" width="400"/>

Create a folder in the Content Drawer for our Brickadia assets to go into

![image](https://user-images.githubusercontent.com/3935931/218207179-e085bd9a-b28e-452b-bb4a-0db4e46af42e.png)


## Importing the Brickadia Model
Import the gLTF file into the level
![image](https://user-images.githubusercontent.com/3935931/218206744-109f7a72-4acf-41e6-8599-8c6dc7001d5c.png)

Select the `.glb` file we created earlier

Choose the Brickadia folder as the location for importing scene content

Change `Offset Uniform Scale` to `0.2` in the Common category

Keep the rest of the settings as the default and hit "Import"

After the shaders/lighting are built, you should be able to see your imported save

![image](https://user-images.githubusercontent.com/3935931/218207711-3594e4c1-db62-4e35-a292-0fab6d286def.png)

## Fixing Materials
As you can probably see, the entire save is white. We need to fix the materials to use the vertex color from the mesh data

Create a new Material Function in the Brickadia folder

<img src="https://user-images.githubusercontent.com/3935931/218208269-d15bd441-f773-4158-b8cb-8bc4e8dd5f6b.png" width="400"/>

Name the Material Function `BrickColor`

Double click the material function to edit it

Using right click to create nodes, and left click to connect them, replicate this Material Function

![image](https://user-images.githubusercontent.com/3935931/218208742-6dbd7828-9ea9-43cf-8a71-641ad46f7eda.png)
Without the sRGBToLinear conversion on the RGB channels, the model will appear washed out and light

Save the material function and exit, make sure `Expose to Library` is checked

![image](https://user-images.githubusercontent.com/3935931/218209283-6b6d943b-26fb-43d8-8e0d-1e974561163e.png)


For each material you wish to fix, edit the base color to use our `BrickColor` function instead of the white
![image](https://user-images.githubusercontent.com/3935931/218209376-b42574ba-d155-48f5-9ba5-f3b1f733301c.png)

As you can see, changing the `BMC_Plastic_Stud_5` material allows us to see color for the brick faces with the stud texture

![image](https://user-images.githubusercontent.com/3935931/218209528-fddf68b4-762c-4594-a843-ec86cf5f18b7.png)

Repeat until color is fixed for all materials

![image](https://user-images.githubusercontent.com/3935931/218209908-811d3e4b-e53f-4d63-9f35-9b7e06abacf5.png)

For Glass materials, set the blend mode to `Translucent`

![image](https://user-images.githubusercontent.com/3935931/218213675-1efc0f89-0390-4ee3-b278-763165791a66.png)

You will have to manually set the `Opacity` for each glass material with a `Constant`

![image](https://user-images.githubusercontent.com/3935931/218213774-05a5446b-b120-4fe8-b600-c88daa306e5a.png)

Here I use `0.1` (90% transparent) for the most intense glass material `BMC_Glass_Bevel_10`, where `10` refers to material intensity

![image](https://user-images.githubusercontent.com/3935931/218213920-c77da01f-a412-4f67-a62e-c540894fec6a.png)


## Moving Around the Model
Move the `Player Start` asset to somewhere that makes sense in the model

When you hit preview you should be able to look around the save with your headset and have hand tracking

[WIP part] adding inputs/blueprints for flying around
