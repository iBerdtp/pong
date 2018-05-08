
## Welcome
Welcome to this tutorial on Processing, one of the easiest and most manageable programming languages if it comes to drawing, based on the Java language.  
My name is Bart van den Broek. Most of you are in their first year of the AI Bachelor, so some of you might know me as one of the TAs for Formal Reasoning and Programming in the first semester. If anything is unclear, you can reach me through email: B.J.vandenBroek@student.ru.nl  
In this tutorial I'm going to assume basic knowledge on Java syntax and built-in functions, and some experience with classes. If you are looking for more tutorials or more information about Processing than I am providing you with or if something's unclear but you don't feel like emailing me, I strongly recommend you to check out Daniel Shiffman's Processing tutorials on his Youtube channel [TheCodingTrain](https://www.youtube.com/user/shiffman/playlists?shelf_id=2&view=50&sort=dd). I have learned most of what I know about Processing from him.
## Getting started
Processing comes with its own IDE, aptly called *Processing*. Visit [the website](www.processing.org) for a download. I will now list some important things to keep in mind, and the most important differences you'll experience being used to Java and Eclipse:  
* **SAVE YOUR SHIT** frequently throughout the process. If you want to shut down your computer, Processing is going to let you, regardless of whether or not that means you'll lose what you've been building for hours. Keep this in mind! I've been using Processing since last summer, and it still happens to me.
* The Processing IDE is a lot more basic. It doesn't come with a built-in dark theme with the joyfully coloured text you've all grown fond of.
* Processing is **very** similar to Java, but it has trimmed off most of the stuff you've got used to, but never *really* saw the point of (except if you've been applying programming on more largely scaled projects), e.g. keywords like `public` and `static`. You can still use these, but you'll rarely need to, and the convention is not to. A function could look as simple as: `void functionName(){}`
* In return for the trivial stuff, Processing comes with a set of nifty functions, mostly centered around drawing. We will encounter these soon.
* In Processing, there is no `main` function. Instead we're going to implement in every file a function called `setup` and one called `draw`. Processing will recognise these and run `setup` once at the start, and then loop `draw` until the application is shut down.
* This creates sort of a tricky situation. The variables you create in `draw` are reset every iteration of `draw`, and you can't reach the variables you created in `setup` via `draw`. Because of this, Processing is very hard to use without global variables, which is why we're going to let you do this. Go wild (and don't tell Franc).
* The origin (with coordinates (0,0)) is in the top-left corner in Processing windows, and the y-axis is flipped upside down compared to the Cartesian Plane you're used to in mathematics.
* We're going to use both variables and constants. Variables and constants are used in almost the same way, only constants are preceded by the keyword `final`, and they can't be altered after their declaration. For the sake of convention, we'll write variables using camelCase, and constants in ALL_CAPS.  
## Pong
To get acquainted, we're now going to recreate Pong, a very basic and old, if not the oldest videogame. In the unlikely case that you haven't heard of this game before, it might be worth Googling, because I'm not going to explain the rules here (or rule, I'm not even sure if there are multiple).  
A lot of the functions we're going to use can be used in a lot more ways than we're going to need now. If you want to learn more about them, don't hesitate to check [their documentation](https://processing.org/reference/)!  
I'll guide you through most of the mechanics, but I'm not giving you all of the code to copy. This tutorial will require some input from your side. Also, I'm not going to tell you how big the puck should be, or how fast it should move. You will have to come up with those values yourself, as to add a bit of personality to the game if you will.
and at the end I'll propose a list of improvements you can try to implement yourself. Also you're encouraged to try and build (parts of) the game without first consulting this tutorial.
### New file
Open up the Processing IDE. You'll see an empty code file called *sketchsomethingsomething*. Start out by **SAVING YOUR SHIT** and naming your file (I suggest you call it Pong). Once you've done this, you can try to run your code. If you do, a small window will pop up. This is going to happen every time you run code in Processing, and in that window we're going to draw all sorts of stuff.  
Be grateful you get to use Processing. Last year we had to use Java Swing, and it took us a couple of sessions before we could make a window appear.
### Setup
We're all very stoked about the window, but it doesn't seem to grant us much space yet. We're going to fix that in the `setup` function:
```Processing
void setup()
{
  size(1200,800);
}
```
Having typed this into your file, you can now expect a larger window to appear when you run your code. (Give it a Whirl.)  
This is all very nice, but since we're going to create Pong, we want to have a black background. Insert into the `setup` function this bit of code: `background(0);`  
This function takes an integer between 0 and 255 inclusive as a parameter, standing for the brightness of the background. It can also take input in other forms. As mentioned before, a lot of the functions we're using can be used in more ways than the ones we're going to use during this tutorial.  
The elements we'll want to draw are going to be white. You can set the colour of the outline of a shape with the `stroke` function, and the colour of the inside with the `fill` function, in a similar fashion as in `background`. Put them in `setup`.
### Draw
It's time to start adding some shapes now. Let's write the `draw` function, and insert this line of code:
```Processing
void draw()
{
  ellipse(width/2, height/2, 200, 100);
}
```
We're sort of taking two steps at the same time here, but I think you'll manage. The `ellipse` function takes four arguments: The first two are the x- and y-coordinates of the center of the ellipse, the last two are the diameters *(not the radii)* of the ellipse in the x and y direction. `width` and `height` are quite obvious. they refer to the width and height of the window.  
Run your code if you will, it's not going to look impressive yet, and it's certainly not interactive. We can change that by rewriting the `ellipse` command to `ellipse(mouseX, mouseY, 200, 100)`. Try it! (Move the mouse around.) Do you notice something odd? You should notice something odd. The ellipse is leaving behind a trail of previously drawn ellipses. This happens because the background is only drawn once in `setup`. This could be an effect you're going for, but not in this tutorial. Let's move `background` to `draw`, and run the code again.  
You might expect the ellipse to flash now. This doesn't happen, because Processing only shows you what it has drawn, after it has finished drawing.
### Pong framework
For Pong, we're not going to use the mouse at all really, so scrap that `ellipse` command for now. We're going to need to think about what should happen every iteration of the game, and then implement those functionalities. This is called *top-down programming*, you may have already heard of it. While we're coding, we're going to add variables and constants when necessary. Some of these are going to be `int`, some are going to be `float`. When in doubt, pick `float`.  
Every iteration, the program should run:  
1. `background` (Draw the background)
2. `handleInput` (Moving the bars up and down the window)
3. `physics` (Some calculations, mostly regarding bouncing)
4. `drawUnits` (Draws the bars and puck to the window) 

Except for the first one, we're going to need to implement these functions ourselves. Let's start with `drawUnits`, because this way we can actually see if our program does what it should do throughout the process.
### drawUnits
Within this function, we're only really going to need three lines of code. We need to draw the puck, draw bar one, and draw bar two.
##### Puck
To draw the puck, we'll make use of the `ellipse` function, so let's start with:
```Processing
void drawUnits()
{
  ellipse(xPuck, yPuck, PUCK_DIAMETER, PUCK_DIAMETER);
}
```
This works, but not if we don't speficy what these variables (or constants) are.  
`PUCK_DIAMETER` can be a constant. We want the puck to be the same size throughout the game. Let's go to the top of the file, and write `final int PUCK_DIAMETER = `, followed by a value you think would be nice (and of course a semicolon). You can change it later, so it doesn't matter if it turns out too big or too small.  
xPuck and yPuck are going to be the coordinates of the center of the puck. These change every iteration of our program, so we can't make them into constants. Below `PUCK_DIAMETER` declare these variables, but don't assign them a value just yet: `float xPuck, yPuck;` They have to be reset every time a point is scored. Let's create a separate function where we initialise these values:
```Processing
void initialise()
{
  xPuck = width/2;
  yPuck = height/2;
}
```
When you can, always refer to existing values like `width` and `height` instead of just filling in the numbers, so that when you change the dimensions of the field, the initial position of the puck also changes. This also makes it so that you don't forget where the hell all those numbers came from again.  
Keep in mind, we're going to call `initialise` every time a point is scored, so later on if we come up with variables that need to be reset every rally, we'll assign them a value in `initialise`.  
Make sure to call `initialise` within `setup`.
##### Bars
Next up, we need to draw the bars controlled by the players on both sides of the field. For this, we'll use a function we haven't seen so far. Let's put this line of code into `drawUnits`:  
```Processing
rect(0, yP1, BAR_WIDTH, BAR_LENGTH);
```
Remember that we had to specify the coordinates of the center of the ellipse, when calling `ellipse`? In `rect` we have to specify the coordinates of the top-left corner of the rectangle. You can change this using `rectMode`, but we're not going to do that now.  
As you can see, we need to set another couple of variables/constants here. Go ahead and add the constants to the list at the top. You can pick a value you think makes sense, and again you could change this later. My advice would be to keep the variables and constants separated for readability and order, but that's up to you.  
`yP1` is going to be the y-coordinate of the top of Player 1's bar. We want to reset the field after every scored point, so like with xPuck and yPuck, let's declare this variable at the top of the file, but assign it a value in `initialise`. What value should this be? Keep in mind. We want the bar to be exactly in the *vertical middle* of the field, and we're talking about the y-coordinate of the *top* of the bar.  
Now we just need to call `rect` one more time for the bar on the right. For this, you need to create `yP2`. See if you can come up with the values yourself (use `width` and `height` and the constants you've already created).
##### First test
Are you calling `background` and `drawUnits` in your `draw` function? Then let's run our code. If everything went according to plan, you'll now see a black window with a puck in the middle, and one rectangle on either side of the field, exactly in the vertical middle.
### handleInput
The next function we're going to write is `handleInput`. Since this is a two-player game, we need two pairs of keys, so we can move both bars up and down. Let's use the arrow keys for the player on the right, and 'w' and 's' for the player on the left.
##### Input framework
The built-in framework of Processing for handling user input is quite laggy for some reason, so Hanna sent me her own solution.  
At the top of the file, in the list of constants, let's add:
```Processing
final boolean[] KEYS = new boolean[255];
```
And at the very bottom of the file, let's add:
```Processing
void keyPressed() { 
  KEYS[keyCode] = true; 
}

void keyReleased() {
  KEYS[keyCode] = false;
}
```
This is going to keep track of what keys are pressed, so whenever we want something to happen on the basis of keyboard input, we just have to consult `KEYS`.
##### handleInput
Within `handleInput`, we can now specify what should happen when one of the keys is pressed:
```Processing
void handleInput()
{
  if(KEYS[87]) // 87 is the keyCode for 'w'
    ...
  if(KEYS[83]) // 83 is the keyCode for 's'
    ...
  if(KEYS[UP])
    ...
  if(KEYS[DOWN])
    ...
}
```
So, what should happen? We need to change the position of the bars with a certain step size every time one of these keys is pressed. Luckily, we just created variables that keep track of the positions of the bars. Fill in the appropriate commands at the `...` and remember that the y-axis is flipped, so the higher a y-coordinate, the lower you get in the window.  
*Hint: Don't just increment the variables by 1. Create a constant for this, and call it `BAR_SPEED`.*  
If you haven't already, go ahead and add `handleInput` to `draw`, and run your code. You'll see you can now control the bars, only they seem to disregard the borders of the field. We'll get to that soon.
### physics
We're down to the last function! However, this is going to be the longest and most complex function. We need to do a couple of things here. You can probably see we're still lacking a lot of the functionality.
##### Moving the puck
Let's start out by allowing the puck to move around at all, and we'll see what difficulties we run into from there. Every iteration of the game, we need to move the puck in both the x- and y-direction. We can create variables for this! Let's call them `vx` and `vy` (and declare them at the top of the file, but don't assign them a value yet).  
*Why shouldn't we make them into constants? Keep in mind that when the puck bounces of a surface, it should change direction, and we can't change constants later on in the code.*  
When a point is scored, we want these values to be reset, and maybe we don't always want them to be the same. That would make for boring gameplay. Where do we put variables that need to be reset every time a point is scored? That's right, in `initialise`.  
I'm starting to feel this tutorial has kind of an increasing *Dora the Explorer* vibe to it, but I'm totally in the zone writing this so tough luck for you.
Within `initialise` we're going to write this bit of code:
```Processing
vx = PUCK_SPEED;
vy = random(-2, 2) * PUCK_SPEED;
```
As you can probably guess, `PUCK_SPEED` is a new constant. Let's add it to the list. I'm also introducing a new function here: `random`. This function takes two parameters, a lower and upper bound, and it returns a random value between those values. You were probably able to figure this out by yourself, but I'm getting paid by the hour so *fuck if I care*.  
This construction makes for a puck that will always take as much time to go from one side of the field to the other, but at a different angle every rally.  
We're ready to create our new function now:
```Processing
void physics()
{
  xPuck += vx;
  yPuck += vy;
}
```
If you run your code now, you'll be pleased to see the puck moving about. If it doesn't, consider adding `physics` to `draw`. You'll be less pleased to see the puck not interacting with the environment whatsoever.  

We're now ready to list the functionalities we still have to implement. We need to:
* constrain the bars;
* let the puck bounce from the borders of the field;
* let the puck bounce from the player controlled bars;
* reinitialise the field if the puck moves past one of the bars.

Let's divide `physics` up into sections separated by comments. You're absolutely free to create separate functions for these tasks if you feel that improves your code.
```Processing
void physics()
{
  // Move puck
  xPuck += vx;
  yPuck += vy;
  
  // Constrain bars
  ...
  
  // Bounce puck from field
  ...
  
  // Bounce puck from bar
  ...
    
  // Scored
  ...
}
```
##### Constraining the bars
Let's give it up for Processing for having created the exact function we need right now, aptly called `constrain`. This function takes three parameters. The first is the value you need constrained, and the last two are the lower an upper bounds respectively, so if some value `x` shouldn't be lower than 1 or higher than 2, you write: `x = constrain(x, 1, 2);`  
Add this function to your code for both players' bar, and remember that the bottom ends of the bars should not be able to cross the bottom of the field (and that the y-axis is still inverted).
##### Bouncing the puck from the borders of the field
Here we need to specify what should happen when the puck leaves the screen. We can write this using an `if`-statement with a single line of code in the body. What should go where? The `if`-statement needs to keep track of two conditions: the top of the puck has left the top of the screen OR the bottom of the puck has left the bottom of the screen. In the event that this happens, the puck's velocity in the x-direction should of course stay the same, and the velocity in the y-direction should flip to its negative equivalent. I'll give away this bit, but you'll have to come up with the code for the ball bouncing off the bars.
```Processing
if(yPuck + R > height || yPuck - R < 0)
    vy *= -1;
```
I created the constant `R` (for radius) and set it to equal `PUCK_RADIUS/2`, because we'll use this value a lot, and using `R`, we'll make the code more readible.  
See if it works!
##### Bouncing the puck from the players' bars
This is going to be a little bit more complicated. Try to figure out this function yourself, and keep in mind that in order for the puck to overlap a bar, it has to be past a certain x-value, and between two y-values, always taking into account the radius of the puck. (You *can* fit this into one `if`-statement, but it will be a very long one.)
##### Reinitialising the field
When the puck has left the field on either players' side, the field should be reinitialised. Luckily we've been keeping track of all the code that does that in `initliase`!  
Pay attention to where in `physics` you're putting the `if`-statement for this bit, and keep in mind that you might need to use `else if` instead of `if`.
### Improvements
Congratulations! Your Pong game should now be up and running, although there are some elements missing or improvable:
* Keeping score  
There is no mechanism that keeps track of the score yet. You can go ahead and create a variable for this, and pick a way to visualise it. You could do this by printing to the console using `System.out.println`, or by using the `text` function, or maybe you have an idea of your own.
* Initial direction  
As of now, the puck will always start of moving to the right. It might be a nice idea to always have the puck start out in the direction of the player that scored the last point, (or take turns or randomise).
* Changing direction during rally  
You could let the puck change direction when it hits a bar, depending on where on the bar it bounces off.
* Speed  
The puck always travels with the initial speed. This may make for long, tiresome rallies. You could speed up the puck every time it hits a bar. This might cause the puck to glitch out of the screen, if your `physics` function isn't robust enough.
* <span style="color:red">Add</span> <span style="color:green">colours</span> <span style="color:blue">yo</span>  
## The end
Congratulations! You've reached the end of this tutorial. All feedback is welcome.
