---
title: Finish the Game of Life!
slug: game-of-life-code
---

Time to code! In this step we are going to hook up the UI we've created in SpriteBuilder with the game logic we're going to code in Xcode.

> [action]
> Open your Xcode project from SpriteBuilder by going to `File > Open Project in Xcode`.

## Create a Grid class

First of all let's create the *Grid* class which we just set as a custom class in SpriteBuilder.

> [action]
> Create a new Swift class in Xcode and make it a subclass of *CCSprite*.  Go to `File > New > File > iOS > Source > Cocoa Touch Class`.
>
> ![image](./GOL-Grid-CCSprite-Swift.png)

## Create a Creature class

We will use some object orientation to implement this game - therefore Creatures will get their own class, too.

> [action]
> Repeat the step above and create a class called *Creature* that also inherits from *CCSprite*.
>
> When you've created the class, open *Creature.swift* and and replace the content of the file with this:
>
>        class Creature: CCSprite {
>
>            var isAlive = false
>            var livingNeighborsCount = 0
>
>            convenience override init() {
>
>            }
>        }

This code will set up two properties which we will be using later to determine if creatures are alive or not and how many neighbors they have. With *init* we declare a custom initializer that sets up the creature with a creature image - we will be implementing this next.

> [action]
> Modify *Creature.swift* by adding the following two lines to init:
>
>        convenience override init() {
>            self.init(imageNamed: "GameOfLifeAssets/Assets/bubble.png")
>            anchorPoint = CGPoint(x: 0, y: 0)
>        }

This initializer simply sets the image of the creature to *bubble.png*. Initializers in Swift must initialize all stored property values. Our initializer doesn't do that - instead it calls a second initializer *self.init(imageNamed:)* that does. We inform the compiler that we'll be calling a second initializer from within our initializer by placing the *convenience* keyword in front of our init declaration.

We are going to implement a second method. Whenever a creature is not alive, we will make it invisible. The easiest way to connect the visibility state to the alive state is to create a callback that is called when the *isAlive* property gets changed. We'll create something called a *proerty observer*.

> [action]
> Modify the *isAlive* property declaration to look like this:
>
>        var isAlive = false {
>           didSet {
>               self.visible = isAlive
>           }
>        }

Now when we set the Creature to be alive it will become visible and when we set it to be dead it will disappear.

## Fill the grid

In this step we are going to implement a method that fills our grid with Creature instances in *Grid.swift*.

> [action]
> Replace the code in *Grid.swift* with the following:
>  
>        let GridRows = 8
>        let GridColumns = 10
>
>        class Grid: CCSprite {
>            var totalAlive = 0
>            var generation = 0
>
>            var cellWidth: CGFloat = 0
>            var cellHeight: CGFloat = 0
>            var gridArray: [[Creature]]!
>
>            // Add Grid methods below here
>        }

Here's what this code does... We define two constants (*GridRows* and *GridColumns*) that describe the amount of rows and columns. Additionally we have a couple of variables. *gridArray* will be a 2d array that will store all the creatures in our grid. *cellWidth* and *cellHeight* will be used to place the creatures on our grid correctly in the method we will be implementing soon. The other two variables *generation* and *totalAlive* will be used to store the current game stats that will be displayed in the game.

Now let's set up the grid! When the Grid class gets loaded a method called *onEnter* is called.

> [action]
> Let's implement this method in *Grid.swift* (after the `// Add Grid methods below here comment`) and call the *setupGrid* method that we are going to write next:
>
>        override func onEnter() {
>            super.onEnter()
>
>            setupGrid()
>
>            userInteractionEnabled = true
>        }

The *onEnter* method is also the right method to activate touch handling on the grid. Later in this tutorial we will handle incoming touch events.

> [action]
> Now let's implement *setupGrid* method after *onEnter*:
>
>        func setupGrid() {
>
>            cellWidth = contentSize.width / CGFloat(GridColumns)
>            cellHeight = contentSize.height / CGFloat(GridRows)
>
>            gridArray = []
>
>            for row in 0..<GridRows {
>                gridArray.append([])
>
>                for column in 0..<GridColumns {
>                    let creature = Creature()
>                    creature.position = CGPoint(x: cellWidth * CGFloat(column),
>                                                 y: cellHeight * CGFloat(row))
>                    addChild(creature)
>
>                    gridArray[row].append(creature)
>
>                    creature.isAlive = false
>                }
>            }
>        }

This code requires some explanation. First we calculate the *cellWidth* and *cellHeight* by dividing the size of the grid by the amount of rows and columns. We iterate through two nested loops and create an array for each row of the grid and fill each row with Creatures. We calculate and assign a position for each creature using the *cellWidth*, *cellHeight*, *row* and *column* variables.

## Test the grid

Now, let's change the method to make sure everything works. Since creatures are only visible when `isAlive` is `true`, change the last line of `setupGrid` from `creature.isAlive = false` to `creature.isAlive = true`. Now we should see all the creatures in our grid!

Run the game to test things out. It should look like this:

![image](./GOL-GridComplete.png)

If your game looks like the image above then everything worked! Set `creature.isAlive` to back to `false` since our test passed.

## Adding user interaction

When the player taps a cell, it should access the Creature that lives on that cell and kill it if it's alive, or bring it to life if its dead!

The good news is that getting touches is very easy! Any class that is a CCNode or inherits from it will automatically call a method called touchBegan when the player touches it. All we need to do is create a touchBegan method and it will get called automatically (because we set *userInteractionEnabled* to *true* earlier)!

> [action]
> Add the following method to your Grid.swift:
>
>        override func touchBegan(touch: CCTouch!, withEvent event: CCTouchEvent!) {
>           let touchLocation = touch.locationInNode(self)
>
>           let creature = creatureForTouchPosition(touchLocation)
>
>           creature.isAlive = !creature.isAlive
>        }

Two things to note here. First, we refer to a method called creatureForTouchPosition which we haven't created yet. We'll do that next! Second, when you set the value for a property you've created, like isAlive, an action called didSet is automatically called. If you don't create one yourself, it'll just set the property to whatever you pass in. In our case, we created a custom one in Creature.swift that not only sets the property, but makes Creatures' visibility change depending on their state as well! This is why your Creatures will automatically go from visible to invisible when you tap on them.

Now let's create *creatureForTouchPosition*.

> [action]
> First, create an empty method:
>
>       func creatureForTouchPosition(touchPosition: CGPoint) -> Creature {
>
>       }

This method takes a parameter (*touchPosition*, which is a CGPoint) and returns a Creature, as indicated by the arrow: `-> Creature`

Divide the y coordinate of the touch (accessed as *touchPosition.y*) by the cellHeight to get the row that was touched. Store that value in an integer called *row*. Divide the x coordinate of the touch (accessed as touchPosition.x) by the cellWidth to get the column that was touched. Store that value in an integer called *column*. Now use *row* and *column* to return the correct Creature from *gridArray*

> [solution]
> The body of `creatureForTouchPosition` should look like this:
>
>        let row = Int(touchPosition.y / cellHeight)
>        let column = Int(touchPosition.x / cellWidth)
>        return gridArray[row][column]

Run your game and try tapping on the Grid. You should see Creatures coming to life and dying where you tap.

## Set Up Your Main Scene

In *MainScene.swift* we will:

- create a timer that will call an *evolveStep* method on the grid and update the stats and gamestate (generation, alive/dead creatures)
- implement a *play* method that starts the timer
- implement a *pause* method that stops the timer

> [action]
> Replace the content of *MainScene.swift* with these lines:
>
>       import Foundation
>
>       class MainScene: CCNode {
>
>          weak var grid: Grid!
>          weak var generationLabel: CCLabelTTF!
>          weak var populationLabel: CCLabelTTF!
>
>          var timer = CCTimer()
>
>          func play() {
>              schedule("step", interval: CCTime(0.5))
>          }
>
>          func pause() {
>              unschedule("step")
>          }
>
>          func step() {
>              grid.evolveStep()
>
>              generationLabel.string = "\(grid.generation)"
>              populationLabel.string = "\(grid.totalAlive)"
>          }
>
>       }

Since we hooked up the play and pause buttons to the play and pause methods in SpriteBuilder, the play and pause methods will automatically get called when we tap them! Note that you cannot run the game before finishing the implementing a few more methods! Move on for more info :)

## Evolve!

Now for the tricky part. We need to implement the *evolveStep* method in *Grid.swift*.

First, add an *evolveStep* method declaration to *Grid.swift*.  It takes no parameters and has no return type.

> [solution]
> `evolveStep` should look like:
>
>       func evolveStep() {
>
>       }

According to the rules of the Game of Life, we need to count how many live neighbors every cell has every step. If it has 0-1 live neighbors the Creature on that cell dies or stays dead. If it has 2-3 live neighbors it stays alive. If it has 4 or more, it stays dead or dies. If it has exactly 3 neighbors and it is dead, it comes to life!

So we need to go through every Creature, count the number of live neighbors it has, and update whether it is alive or dead.

> [action]
> Fill your *evolveStep* method with the following code:
>
>       //update each Creature's neighbor count
>       countNeighbors()
>
>       //update each Creature's state
>       updateCreatures()
>
>       //update the generation so the label's text will display the correct generation
>       generation++

Once we fill out the *countNeighbors* and *updateCreatures* methods, we're done!

Declare *countNeighbors* in your Grid.swift. It takes no parameters and has no return type.

> [solution]
> `countNeighbors` should look like:
>
>        func countNeighbors() {
>
>        }

Let's fill it in!

> [action]
> Fill it with the following code:
>
>        for row in 0..<gridArray.count {
>            for column in 0..<gridArray[row].count {
>
>                let currentCreature = gridArray[row][column]
>                currentCreature.livingNeighborsCount = 0
>
>                for x in (row - 1)...(row + 1) {
>                    for y in (column - 1)...(column + 1) {
>
>                        let validIndex = isValidIndex(x: x, y: y)
>
>                        if validIndex && !(x == row && y == column) {
>
>                            let neighbor = gridArray[x][y]
>
>                            if neighbor.isAlive {
>                                currentCreature.livingNeighborsCount++
>                            }
>                        }
>                    }
>                }
>            }
>         }

You'll notice that the above code refers to a method called `isValidIndex:(x: x, y: y)`.

> [action]
> Add `isIndexValid` to the `Grid` class:
>
>        func isValidIndex(x x: Int, y: Int) -> Bool {
>           return !(x < 0 || y < 0 || x >= GridRows || y >= GridColumns)
>        }

<!-- MAKE SCHOOL -->

Now it's your turn to write *updateCreatures*. Create it in *Grid.swift*. You will need to create a double-nested for-loop like we did in *countNeighbors* to access every creature in the Grid. Look over the code in *countNeighbors* if you need a refresher on how to do that.

Next you need to create an `if/else-if statement`. In the `if statement`, check if the Creature's *livingNeighbors* property is set to 3. If it is, that means it has 3 live neighbors so you want to set its *isAlive* property to true. In the `else-if` you want to check if the Creature has less than or equal to 1 living neighbors or more than or equal to 4. If either are true, set the Creature's *isAlive* property to false.

> [solution]
> `updateCreatures` should look like this:
>
>        func updateCreatures() {
>          for row in 0..<gridArray.count {
>            for column in 0..<gridArray[row].count {
>              let currentCreature = gridArray[row][column]
>              let liveNeighbors = currentCreature.livingNeighborsCount
>              if liveNeighbors == 3 {
>                currentCreature.isAlive = true
>              } else if liveNeighbors <= 1 || liveNeighbors >= 4 {
>                currentCreature.isAlive = false
>              }
>            }
>          }
>        }

Once you've completed *updateCreatures*, run your game and see what happens. If you populate some cells and then tap start, you should see the game run properly. Try some of [these popular patterns](http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life#Examples_of_patterns) and see if they behave as expected.

The only thing that should be missing is the count of live Creatures. To make the label update properly, reset the *totalAlive* property at the beginning of your *updateCreatures* method by setting it to 0. Create an if statement at the end of the for loop that checks if creatures are alive, if they are you need to increment *totalAlive* by 1.

> [solution]
> `updateCreatures` should now look like this:
>
>        func updateCreatures() {
>          totalAlive = 0
>          for row in 0..<gridArray.count {
>            for column in 0..<gridArray[row].count {
>              let currentCreature = gridArray[row][column]
>              let liveNeighbors = currentCreature.livingNeighborsCount
>              if liveNeighbors == 3 {
>                currentCreature.isAlive = true
>              } else if liveNeighbors <= 1 || liveNeighbors >= 4 {
>                currentCreature.isAlive = false
>              }
>
>              if currentCreature.isAlive {
>                totalAlive++
>              }
>            }
>          }
>        }

Run the game again - you should be done!
