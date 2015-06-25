---
title: What You Learned!
slug: game-of-life-code
---  

#Designing for Compatibility

* 	**Using % position:** Ensure an element is in the correct screen alignment, regardless of device.
*	**Width inset in points:** Being able to define a width inset allows you to utilize remaining screen with a static sprite.
* 	**Using layout box:** Using a layout box allows you to quickly and easily display objects of all sizes.
*  **The importance of anchorpoints:** When designing for compatibility, you must be aware of how the anchor point affects the sprite so it adjusts properly on all screen sizes.

#Using Selectors, UserInteraction, and 2D Arrays

*	**Scheduling the grid update:** You can schedule a selector to act just like update does, but with the freedom of running it at any interval of time you want. This is ideal for things that need to be updated regularly, but not every frame.
* 	**Using touch information:** In order to add creatures to the grid, we needed to locate user touches in the grid. To do that, we used touch.locationInNode(self). This is the easiest way to find exactly where the touch has occurred.
*  	**2-Dimensional Arrays:** 2D are arrays within arrays. They're very useful when storing representations of game objects; our grid is a perfect example a 2D array use case because it is visually 2-dimensional. 

