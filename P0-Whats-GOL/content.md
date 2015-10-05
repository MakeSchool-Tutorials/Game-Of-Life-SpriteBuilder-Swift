---
title: What's the Game of Life (in Swift)?
slug: what-game-of-life
---

In this tutorial you are going to further familiarize yourself with Swift and SpriteBuilder by making a mobile version of Conway's Game of Life. If you've never heard of Conway's Game of Life, [Wikipedia](http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) has a great article. Nearly every programmer has written a version of this game at some point in their careers and wasted lots of time staring at cool shapes morphing. Consider this your initiation :p

#TL;DR

There is a grid of cells. A cell is either alive or dead. If a cell has less than two live neighbors, it dies. If it has more than three neighbors, it dies. If a live cell has exactly two or three neighbors, it stays alive and if a dead cell has exactly three neighbors, it comes to life.

You can check out an in-browser version of the Game Of Life [here](https://jsfiddle.net/makeschool_dion/zose7rv3/embedded/result/)!

Try placing a few live cells and then hitting the next button to run one round. The Wikipedia article has some great examples of common patterns that produce cool effects. Press the animate button to continuously run the game. Play around with it a little and come back when you're ready. We'll wait :)

#What you will learn

In this tutorial, you will learn how to:

- Use layout boxes in SpriteBuilder and Cocos2D
- Subclass CCSprite without setting up a CCB file
- Programmatically generate a grid of sprites
- Handle touch input
- Translate game logic into code

We'll touch on many more topics but these are some of the most important ones.

#The finished product

We'll be making a mobile version of Conway's Game of Life. The final game will look like this:

<!-- TODO: update with gif -->

![Complete grid](./GOL-GridComplete.png)

Let's get started with a blank SpriteBuilder project!

#Feedback

If you have feedback on this tutorial or find any mistakes, please open issues on the [GitHub Repository](https://github.com/MakeSchool-Tutorials/Game-Of-Life-SpriteBuilder-Swift).
