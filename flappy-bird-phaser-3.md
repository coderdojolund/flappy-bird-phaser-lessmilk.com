# How to Make Flappy Bird in HTML5 With Phaser &ndash; Part 3

![](http://lessmilk.com/imgtut/FB3/1.png)

In [Part 1](/tutorial/flappy-bird-phaser-1) of this tutorial we created a very basic Flappy Bird clone, and in [Part 2](/tutorial/flappy-bird-phaser-2) we made it more interesting with animations and sounds. In this last part we are going to make the game mobile friendly, and you will see that this is really simple to do with Phaser.

## Mobile Testing

Before making any change to our project it's important to know how to test a mobile game on desktop. Here's how you can do so in Google Chrome, though you can probably do something similar in other browsers.

Launch Google Chrome, open the devtools (top menu > view > developer > developer tools) and click on the tiny mobile icon.

![](http://lessmilk.com/imgtut/FB3/2.png)

And now you can change the values at the top of the screen to emulate different mobile devices. Just make sure to reload the page every time you change the settings.

![](http://lessmilk.com/imgtut/FB3/3.png)

As you can see our game loads on an iPhone 5, but:

*   We don't see the whole game (it's cropped on the right).
*   There's a weird white border on the left.
*   It's not centred on the screen vertically.
*   And we can't use the spacebar to jump.

Let's fix all of this.

## Scaling

Let's start by properly scaling our game on the screen.

Open the index.html file and add this between the 2 `head` tags.

    <meta name="viewport" content="initial-scale=1.0" />

    <style> 
    * {
        margin: 0;
        padding: 0;
    }
    </style> 

The first line will make the page mobile friendly, and the rest will simply remove all margins and padding the HTML elements might have.

Next, open the main.js file and add this code at the beginning of the `create()` function.

    // If this is not a desktop (so it's a mobile device) 
    if (game.device.desktop == false) {
        // Set the scaling mode to SHOW_ALL to show all the game
        game.scale.scaleMode = Phaser.ScaleManager.SHOW_ALL;

        // Set a minimum and maximum size for the game
        // Here the minimum is half the game size
        // And the maximum is the original game size
        game.scale.setMinMax(game.width/2, game.height/2, 
            game.width, game.height);

        // Center the game horizontally and vertically
        game.scale.pageAlignHorizontally = true;
        game.scale.pageAlignVertically = true;
    }

The `game.scale.setMinMax()` is optional, but it's a good practice to use it to make sure the game is neither too small nor too big.

Note that you can put the 2 last lines of code outside of the `if` condition to have the game centered on desktop too.

You should now see the game taking the whole width on all mobile devices (up to the maximum game size we defined).

![](http://lessmilk.com/imgtut/FB3/4.png)

## Jumping

The last step is to be able to make the bird jump when touching the screen. This can be done by adding a single line of code in the `create()` function.

     // Call the 'jump' function when we tap/click on the screen
    game.input.onDown.add(this.jump, this);

![](http://lessmilk.com/imgtut/FB3/5.gif)

And now you can play the Flappy Bird clone on desktop and mobile devices :-)

## Conclusion

In about 10 lines of code we managed to make our game mobile friendly, that's pretty cool!

This was the last part of the Flappy Bird tutorial. If you want to learn more about Phaser, you should check out my ebook Discover Phaser. It explains in detail how to build a full featured game with Phaser. More information on [DiscoverPhaser.com](http://www.discoverphaser.com).

[![](http://lessmilk.com/img/phaserbook.jpg)](http://www.discoverphaser.com)
