# How to Make Flappy Bird in HTML5 With Phaser - Part 2

![](http://lessmilk.com/imgtut/FB2/1.png)

In the [first part](http://lessmilk.com/tutorial/flappy-bird-phaser-1) of this tutorial we did a simple Flappy Bird clone. It was nice, but quite boring to play. In this part we will see how to add animations and sounds. We won't change the game's mechanics, but the game will feel a lot more interesting.

Open the main.js file that we created in the last part, and we are good to go.

## Add Fly Animation

The bird is moving up and down in a quite boring way. Let's improve that by adding some animations like in the original game.

![](http://lessmilk.com/imgtut/FB2/5.gif)

You can see that:

*   The bird slowly rotates downward, up to a certain point.
*   And when the bird jumps, it rotates upward.

The first one is easy. We just need to add this in the `update()` function.

    if (this.bird.angle < 20)
        this.bird.angle += 1; 

For the second one, we could simply add `this.bird.angle = -20;` in the `jump()` function. However, changing instantly the angle will look weird. Instead, we are going to make the bird change its angle over a short period of time. We can do so by creating an animation in the `jump()` function.

    // Create an animation on the bird
    var animation = game.add.tween(this.bird);

    // Change the angle of the bird to -20° in 100 milliseconds
    animation.to({angle: -20}, 100);

    // And start the animation
    animation.start(); 

For your information, the preceding code can be rewritten in a single line like this.

    game.add.tween(this.bird).to({angle: -20}, 100).start(); 

If you test the game right now, you will notice that the bird is not rotating like the original Flappy Bird. It's rotating like the drawing on the left, and we want it to look like the one on the right.

![](http://lessmilk.com/imgtut/FB2/2.png)

What we need to do is change the center of rotation of the bird (the red dot above) called "anchor". So we add this line of code in the `create()` function.

     // Move the anchor to the left and downward
    this.bird.anchor.setTo(-0.2, 0.5); 

If you test the game now, the animation should look a lot better.

![](http://lessmilk.com/imgtut/FB2/3.gif)

## Add Dead Animation

When the bird dies, we restart the game instantly. Instead, we are going to make the bird fall off the screen.

First, we update this line of code in the `update()` function to call `hitPipe()` instead of `restartGame()` when the bird hit a pipe.

    game.physics.arcade.overlap(
        this.bird, this.pipes, this.hitPipe, null, this);  

Now we create the new `hitPipe()` function.

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

Last thing, we don't want to be able to make the bird jump when it's dead. So we edit the `jump()` by adding this 2 lines at the beginning of the function.

    if (this.bird.alive == false)
        return;  

And we are done adding animations.

![](http://lessmilk.com/imgtut/FB2/4.gif)

## Add Sound

Adding sounds is super easy with Phaser.

We start by loading the jump sound in the `preload()` function.

    game.load.audio('jump', 'assets/jump.wav'); 

Now we add the sound in the game by putting this in the `create()` function.

    this.jumpSound = game.add.audio('jump'); 

Finally we add this line in the `jump()` function to actually play the sound effect.

    this.jumpSound.play(); 

And that's it, we now have animations and sounds in the game!

## What's Next

In only a few lines of code we managed to make the game a lot better, that's the power of Phaser. In the next part of this tutorial we will see how to make this game mobile friendly. [Read Part 3](http://lessmilk.com/tutorial/flappy-bird-phaser-3).

For your information, I also wrote a book on how to make a full featured game with Phaser. More information on: [DiscoverPhaser.com](http://www.discoverphaser.com).
