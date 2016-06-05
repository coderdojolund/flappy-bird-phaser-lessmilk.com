# Så här gör du Flappy Bird i HTML5 med Phaser &ndash; Del 3
[_Det engelska originalet hittar du här_](http://www.lessmilk.com/tutorial/flappy-bird-phaser-3)
![](http://lessmilk.com/imgtut/FB3/1.png)

I [Del 1](flappy-bird-phaser-1.md) av den här handledningen skapade vi en mycket basal Flappy Bird-klon och i  [Del 2](flappy-bird-phaser-2.md) gjorde vi den intressantare med animeringar och ljud. I den här sista delen ska vi göra spelet mobilvänligt och du kommer att inse att detta är riktigt lätt att göra med Phaser.

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

## Skalning

Vi börjar med att få rätt på spelets skalning på skärmen.

Öppna filen `index.html` och lägg till detta mellan de två `head`-taggarna.

    <meta name="viewport" content="initial-scale=1.0" />

    <style> 
    * {
        margin: 0;
        padding: 0;
    }
    </style> 

Den första raden gör spelet mobilvänligt och resten tar helt enkelt bort alla marginaler och utfyllnad som HTML-elementen kan ha.

Öppna sen filen `main.js` och lägg till den här koden i början av funktionen `create()`.

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

Raden `game.scale.setMinMax()` är frivillig, men det är god praxis att använda detta för att få spelet lagom stort på skärmen.

Lägg märke till att du kan ha de två sista raderna utanför `if`-villkoret om du vill ha spelet centrerat på desktopversionen också.

Du bör nu se att spelet tar upp hela bredden på alla mobila enheter (upp till maximala spelstorleken vi definiferade).

![](http://lessmilk.com/imgtut/FB3/4.png)

## Att hoppa

Det sista steget är att göra det möjligt att hoppa med fågeln när vi rör skärmen. Detta kan göras genom att lägga till en enda kodrad i funktionen `create()`.

     // Call the 'jump' function when we tap/click on the screen
    game.input.onDown.add(this.jump, this);

![](http://lessmilk.com/imgtut/FB3/5.gif)

Och nu kan du spelet Flappy Bird-klonen på desktop och på mobila enheter :-)

## Slutsats

Med cirka 10 rader kod lyckades vi göra vårt spel mobilvänligt; det är rätt coolt!

Detta var sista delen av Flappy Bird-handledningen. Om du vill lära dig mer om Phaser, kolla in min e-bok Discover Phaser. Den förklarar i detalj hur man bygger en fullfjädrat spel med Phaser. Mer info på [DiscoverPhaser.com](http://www.discoverphaser.com).

[![](http://lessmilk.com/img/phaserbook.jpg)](http://www.discoverphaser.com)

[_Det engelska originalet hittar du här_](http://www.lessmilk.com/tutorial/flappy-bird-phaser-3)
