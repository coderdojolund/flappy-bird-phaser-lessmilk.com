# Så här gör du Flappy Bird i HTML5 med Phaser &ndash; Del 1

![](http://lessmilk.com/imgtut/FB1/1.png)

Flappy Bird är ett trevligt litet spel med lättbegriplig mekanik, och jag tyckte att det kunde vara perfekt för en handledning i HTML5-spel för nybörjare. Vi ska bygga en förenklad version av Flappy Bird med bara 65 rader JavaScript med ramverket Phaser.

Phaser är ett gratis, fantastiskt öppen källkods-ramverk för att göra spel som fungerar i alla webbläsare.

Om du vill spela spelet som vi ska bygga,  [klicka här](http://lessmilk.com/game/flappy-bird/). Tryck på mellanslag eller på spelytan för att hoppa.

## Sätta igång

För att starta handledningen behöver du ladda ner [den här tomma mallen](http://lessmilk.com/content/flappy-bird/empty.zip) som jag har förberett. I den hittar du

*   phaser.min.js, Phaser-ramverket v2.4.3.
*   index.html, där spelet visas på skärmen.
*   main.js, en fil där vi skriver all vår kod.
*   assets/, en mapp med två bilder och en ljudeffekt.

![](http://lessmilk.com/imgtut/FB1/7.png)

## Tomt projekt

Det första vi ska göra är att bygga ett tomt projekt.

Öppna filen `index.html` och lägg till den här koden.

    <!DOCTYPE html>  
    <html>
        <head>  
            <meta charset="utf-8" />
            <title> Flappy Bird Clone </title>
            <script type="text/javascript" src="phaser.min.js"></script>
            <script type="text/javascript" src="main.js"></script>
        </head>

        <body>

        </body>  
    </html>  

Den laddar helt enkelt våra två JavaScript-filer.

Vi lägger till detta i filen `main.js` för att skapa ett tomt Phaser-spel.

    // Create our 'main' state that will contain the game
    var mainState = {
        preload: function() { 
            // This function will be executed at the beginning     
            // That's where we load the images and sounds 
        },

        create: function() { 
            // This function is called after the preload function     
            // Here we set up the game, display sprites, etc.  
        },

        update: function() {
            // This function is called 60 times per second    
            // It contains the game's logic   
        },
    };

    // Initialize Phaser, and create a 400px by 490px game
    var game = new Phaser.Game(400, 490);

    // Add the 'mainState' and call it 'main'
    game.state.add('main', mainState); 

    // Start the state to actually start the game
    game.state.start('main');

Det enda vi behöver för att göra ett spel med Phaser är att fylla i funktionerna `preload()`, `create()` och `update()`.

## Fågeln

Vi koncentrerar oss först på att lägga till en fågel i spelet som hoppar när vi trycker på mellanslag.

Allt är rätt enkelt och välkommenterat, så du bör kunna förstå koden nedan. För bättre läsbarhet har jag tagit bort Phasers kod för initiering och tillståndshantering, som du kan se ovan.

Först uppdaterar vi funktionerna `preload()`, `create()` och `update()`.

    preload: function() { 
        // Load the bird sprite
        game.load.image('bird', 'assets/bird.png'); 
    },

    create: function() { 
        // Change the background color of the game to blue
        game.stage.backgroundColor = '#71c5cf';

        // Set the physics system
        game.physics.startSystem(Phaser.Physics.ARCADE);

        // Display the bird at the position x=100 and y=245
        this.bird = game.add.sprite(100, 245, 'bird');

        // Add physics to the bird
        // Needed for: movements, gravity, collisions, etc.
        game.physics.arcade.enable(this.bird);

        // Add gravity to the bird to make it fall
        this.bird.body.gravity.y = 1000;  

        // Call the 'jump' function when the spacekey is hit
        var spaceKey = game.input.keyboard.addKey(
                        Phaser.Keyboard.SPACEBAR);
        spaceKey.onDown.add(this.jump, this);     
    },

    update: function() {
        // If the bird is out of the screen (too high or too low)
        // Call the 'restartGame' function
        if (this.bird.y < 0 || this.bird.y > 490)
            this.restartGame();
    },

Och direkt nedanför den koden lägger till två nya funktioner.

    // Make the bird jump 
    jump: function() {
        // Add a vertical velocity to the bird
        this.bird.body.velocity.y = -350;
    },

    // Restart the game
    restartGame: function() {
        // Start the 'main' state, which restarts the game
        game.state.start('main');
    },

## Testning

Att köra ett Phaser-spel direkt i webbläsaren fungerar inte, eftersom JavaScript inte får läsa filer från ditt lokala filsystem. FÖr att lösa det behöver vi en webbläsare för att spela och testa vårt spel.

Det finns många sätt att ha en lokal webbserver på en dator och vi går snabbt igenom tre här nedan.


* Använd Brackets. Öppna mappen som innehåller spelet i [Brackets editor](http://brackets.io/) och klicka på den lilla blixtikonen som finns uppe i högra hörnet i fönstret. Detta öppnar direkt din webbläsare med en aktuell förhandsgranskning från en webbserver. Det är förmodligen den enklaste lösningen.
* Använd appar. Du kan ladda ner [WAMP](http://www.wampserver.com/en/) (Windows) eller [MAMP](https://www.mamp.info/en/) (Mac). Båda har rena användargränssnitt och enkla installationsanvisningar finns. 
* Använd kommandoraden. Om du har Python installerat och är bekant med kommandoraden, skriv `python -m SimpleHTTPServer` för att starta en webbläsare som kör i aktuell mapp. Använd sen URL:en 127.0.0.1:8000 för att spela spelet.

När du är klar, bör du se det här på skärmen.

![](http://lessmilk.com/imgtut/FB1/3.gif)

## Rören

Ett Flappy Bird-spel utan några hinder (de gröna rören) är inte värst intressant, så nu ändrar vi på det.

Börja med att ladda rör-sprajten i funktionen `preload()`.

    game.load.image('pipe', 'assets/pipe.png');

Eftersom vi kommer att hantera många rör i spelet, är det lättare att använda en Phaser-funktion som heter "group". Gruppen kommer helt enkelt att innehålla alla våra rör. För att skapa gruppen, lägger vi till detta i vår funktion `create()`.

    // Create an empty group
    this.pipes = game.add.group(); 

Now we need a new function to add a pipe in the game. We can do that with a new function.

    addOnePipe: function(x, y) {
        // Create a pipe at the position x and y
        var pipe = game.add.sprite(x, y, 'pipe');

        // Add the pipe to our previously created group
        this.pipes.add(pipe);

        // Enable physics on the pipe 
        game.physics.arcade.enable(pipe);

        // Add velocity to the pipe to make it move left
        pipe.body.velocity.x = -200; 

        // Automatically kill the pipe when it's no longer visible 
        pipe.checkWorldBounds = true;
        pipe.outOfBoundsKill = true;
    },

The previous function creates one pipe, but we need to display 6 pipes in a row with a hole somewhere in the middle. So let's create a new function that does just that.

    addRowOfPipes: function() {
        // Randomly pick a number between 1 and 5
        // This will be the hole position
        var hole = Math.floor(Math.random() * 5) + 1;

        // Add the 6 pipes 
        // With one big hole at position 'hole' and 'hole + 1'
        for (var i = 0; i < 8; i++)
            if (i != hole && i != hole + 1) 
                this.addOnePipe(400, i * 60 + 10);   
    },

Here's an image to make things more clear, for when `hole = 2`.

![](http://lessmilk.com/imgtut/FB1/6.png)

To actually add pipes in our game we need to call the `addRowOfPipes()` function every 1.5 seconds. We can do this by adding a timer in the `create()` function.

    this.timer = game.time.events.loop(1500, this.addRowOfPipes, this); 

Now you can save your file and test the code. This is slowly starting to look like a real game.

![](http://lessmilk.com/imgtut/FB1/4.gif)

## Scoring and Collisions

The last thing we need to finish the game is adding a score and handling collisions. And this is quite easy to do.

We add this in the `create()` function to display the score in the top left.

    this.score = 0;
    this.labelScore = game.add.text(20, 20, "0", 
        { font: "30px Arial", fill: "#ffffff" });   

And we put this in the `addRowOfPipes()`, to increase the score by 1 each time new pipes are created.

    this.score += 1;
    this.labelScore.text = this.score;  

Next, we add this line in the `update()` function to call `restartGame()` each time the bird collides with a pipe from the `pipes` group.

    game.physics.arcade.overlap(
        this.bird, this.pipes, this.restartGame, null, this);

And we are done! Congratulations, you now have a Flappy Bird clone in HTML5\.

![](http://lessmilk.com/imgtut/FB1/5.gif)

## What's Next?

The game is working but it's a little bit boring. In the next part of this tutorial we'll see how we can make it better by adding sounds and animations. [Read Part 2](http://lessmilk.com/tutorial/flappy-bird-phaser-2).

For your information, I also wrote a book on how to make a full featured game with Phaser. More information on [DiscoverPhaser.com](http://www.discoverphaser.com).
