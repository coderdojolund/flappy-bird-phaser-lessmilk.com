#Så här gör du Flappy Bird i HTML5 med Phaser &ndash; Del 2
[_Det engelska originalet hittar du här_](http://www.lessmilk.com/tutorial/flappy-bird-phaser-2)
![](http://lessmilk.com/imgtut/FB2/1.png)

I [första delen](flappy-bird-phaser-1.md) av den här handledningen gjorde vi en enkel Flappy Bird-klon. Den var trevlig, men ganska tråkig att spela. I den här delen ska vi se hur man lägger till animeringar och ljud. Vi ändrar inte spelets mekanik, men spelet kommer att kännas mycket mer intressant.

Öppna filen `main.js`som vi skapade i senaste delen, så är vi redo att sätta igång.

## Lägg till flyktanimering

Fågeln rör sig upp och ner på ett rätt tråkigt sätt. Vi förbättrar det genom att lägga till några animeringar som i originalspelet.

![](http://lessmilk.com/imgtut/FB2/5.gif)

Du kan se att:

* Fågeln sakta roterar neråt, till ett visst läge.
* Och när fågeln hoppar, roterar den uppåt.

Den första biten är lätt. Vi behöver bara lägga till detta i funktionen `update()`.

    if (this.bird.angle < 20)
        this.bird.angle += 1; 

För andra biten skulle vi helt enkelt kunna lägga till `this.bird.angle = -20;` i funktionen `jump()`. Men att plötsligt ändra vinkeln ser knasigt ut. Istället ska vi få fågeln att ändra sin vinkel under kort tid. Vi kan göra det genom att skapa en animering i funktionen `jump()`.

    // Create an animation on the bird
    var animation = game.add.tween(this.bird);

    // Change the angle of the bird to -20° in 100 milliseconds
    animation.to({angle: -20}, 100);

    // And start the animation
    animation.start(); 

För kännedom kan koden ovan skrivas som en enda rad så här.

    game.add.tween(this.bird).to({angle: -20}, 100).start(); 

Om du testar spelet just nu, kommer du att se att fågeln inte roterar som Flappy Bird i original. Den roterar som ritningen till vänster, och vi vill att det ska se ut som den till höger.

![](http://lessmilk.com/imgtut/FB2/2.png)

Det vi behöver göra är att ändra rotationscentrum på fågeln (röda punkten ovan), som kallas "anchor" (ankare). Så vi lägger till den här kodraden i funktionen `create()`.

     // Move the anchor to the left and downward
    this.bird.anchor.setTo(-0.2, 0.5); 

Om du testar spelet nu, bör animeringen se mycket bättre ut.

![](http://lessmilk.com/imgtut/FB2/3.gif)

[Här ser du kodändringarna för det här avsnittet](https://github.com/coderdojolund/flappy-bird-phaser-lessmilk.com/compare/1.4_Scoring_and_collisions...2.0_Flight_animation)

## Lägg till dödsanimering

När fågeln dör, startar vi om spelet direkt. Istället ska vi få fågeln att ramla nerför skärmen.

Först ändrar vi den här kodraden i `update()`-funktionen så att den anropas `hitPipe()` istället för `restartGame()` när fågeln träffar ett rör.

    game.physics.arcade.overlap(
        this.bird, this.pipes, this.hitPipe, null, this);  

Nu skapar vi den nya funktionen `hitPipe()`.

    hitPipe: function() {
        // If the bird has already hit a pipe, do nothing
        // It means the bird is already falling off the screen
        if (this.bird.alive == false)
            return;

        // Set the alive property of the bird to false
        this.bird.alive = false;

        // Prevent new pipes from appearing
        game.time.events.remove(this.timer);

        // Go through all the pipes, and stop their movement
        this.pipes.forEach(function(p){
            p.body.velocity.x = 0;
        }, this);
    }, 

Slutligen så vill vi inte kunna få fågeln att hoppa när den är död. Så vi ändrar `jump()` genom att lägga till de här två raderna i funktionens början.

    if (this.bird.alive == false)
        return;  

Och så var vi klara med att lägga till animeringar.

![](http://lessmilk.com/imgtut/FB2/4.gif)

[Här ser du kodändringarna för det här avsnittet](https://github.com/coderdojolund/flappy-bird-phaser-lessmilk.com/compare/2.0_Flight_animation...2.1_Death_animation)

## Lägg till ljud

Att lägga till ljud är busenkelt med Phaser.

Vi börjar med att ladda hoppljudet i funktionen `preload()`.

    game.load.audio('jump', 'assets/jump.wav'); 

Nu lägger vi till ljudet i spelet genom att ha följande i `create()`-funktionen.

    this.jumpSound = game.add.audio('jump'); 

Slutligen lägger vi till den här raden i funktionen `jump()` för att faktiskt spela ljudeffekten.

    this.jumpSound.play(); 

Och det var det; nu har vi animeringar och ljud i spelet.

[Här ser du kodändringarna för det här avsnittet](https://github.com/coderdojolund/flappy-bird-phaser-lessmilk.com/compare/2.1_Death_animation...2.2_Sound)

## Vad händer nu?

Med bara några få kodrader har vi lyckats göra spelet mycket bättre; det är styrkan med Phaser. I nästa del av den här handledningen ska vi se hur vi gör spelet mobilvänligt. [Läs Del 3](flappy-bird-phaser-3.md).

För kännedom har jag också skrivit en bok om hur man gör ett fullfjädrat spel med Phaser. Mer information på [DiscoverPhaser.com](http://www.discoverphaser.com).

[_Det engelska originalet hittar du här_](http://www.lessmilk.com/tutorial/flappy-bird-phaser-2)
