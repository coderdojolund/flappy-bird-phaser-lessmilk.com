# Så här gör du Flappy Bird i HTML5 med Phaser &ndash; del 3

![](http://lessmilk.com/imgtut/FB3/1.png)

I [Del 1](http://lessmilk.com/tutorial/flappy-bird-phaser-1) av den här handledningen skapade vi en mycket basal Flappy Bird-klon och i  [Del 2](http://lessmilk.com/tutorial/flappy-bird-phaser-2) gjorde vi den intressantare med animeringar och ljud. I den här sista delen ska vi göra spelet mobilvänligt och du kommer att inse att detta är riktigt lätt att göra med Phaser.

## Mobiltestning

Innan vi gör några ändringar i projektet är det viktigt att veta hur man testar ett mobilspel på desktop. Så här kan du göra i Google Chrome, fast du kan säkert göra något liknande i andra webbläsare.

Starta Google Chrome, öppna utvecklarverktygen (toppmenyn > Fler verktyg > Verktyg för programmerare eller Ctrl-Shift I).

![](http://lessmilk.com/imgtut/FB3/2.png)

Och nu kan du ändra värdena högst upp i programmerarfönstret för att emulera olika mobila enheter. Tänk bara på att ladda om sidan varje gång du ändrar inställningarna. 

![](http://lessmilk.com/imgtut/FB3/3.png)

Som du kan se så laddas vårt spel på en iPhone 5, men
* Vi ser inte hela spelet (det är avklippt till höger).
* Det är en konstig vit kant till vänster.
* Det är inte vertikalt centrerat på skärmen.
* Och vi kan inte använda mellanslag för att hoppa.

Nu fixar vi allt det.

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
