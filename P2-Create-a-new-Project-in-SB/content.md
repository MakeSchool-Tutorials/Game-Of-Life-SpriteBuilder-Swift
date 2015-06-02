---
title: Create a New Project in SpriteBuilder
slug: create-new-project-spritebuilder
---

Let's get to work by creating a new SpriteBuilder project:

![image](./Spritebuilder_NewProject.png)

Name your new project “GameOfLife”. Make sure you select Swift as your primary language:

![image](./GOL-NewProject-Swift.png)

Once the project is created, open
Finder and take a look at the folder structure of your project. If you
can't find your project in Finder, search for it using Spotlight at the
top right of your screen.
To open the folder, right click and Show Package Contents:

![image](./GOL-ShowPackageContents-Swift.png)


The folder structure should look something
like this:

![image](./sbGoLCreation.png)

SpriteBuilder created a new folder (GameOfLife.spritebuilder). Inside it
are a SpriteBuilder project (GameOfLife.ccbproj) and a new Xcode project
(GameOfLife.xcodeproj).

The default SpriteBuilder project comes with one scene (MainScene). Your
empty project should look like this:

![image](./Sprtebuilder_EmptyProject.png)

Testing your setup
==================

Let’s make sure everything was generated correctly. Click the publish
button (on left side of the toolbar) in SpriteBuilder:

![image](./Spritebuilder_Publish.png)

Now open GameOfLife.xcodeproj to view the project in Xcode. Run this on
the iPhone simulator and verify it shows the “SpriteBuilder” message:

![image](./Spritebuilder_Xcode_Launch.png)

Congratulations! Now you have run your first SpriteBuilder project and
you know your tools are working.
