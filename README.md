# javascript-state-management

Boilerplate for canvas
Add dogImage img tag and h1 tag with id loading and text
Add type module to script(allows you to split code into separate files and use import/export keywords to connect with the data)
The above will be useful when connecting with different js files(as you can see with the multiple js files in this project)
Set up the css file(background will be blue)
Similar to previous projects, you will set up canvas but this time since you have a loading transition which is visually shown on screen you will need to create a loading variable and set its display to none once the load event listener finish loading the page
Once you see the color blue taking up the whole canvas, you can remove the blue color from css
Now, let's work on Player class in player.js file
As always, take note of what the Player class should be limited by
In the game, Player should be restricted from moving pass the canvas so add the parameters in the constructor(namely gameWidth and gameHeight in this case) and set them as class properties
In this project, we are dealing with states, so add a states property in Player and give it an empty array
Since Player can only be in one state at any given time, add a currentState property to keep track of it
currentState will be set to index 0 of states array for now
Add image property with getElementById or use its id directly(js DOM is created upon id creation)
Add the width and height of the sprites you are using
Width is total width divided by the number of sprites in a row(1800/9)
Height is total height divided by number of sprites in a column(2182/12)
Set xy position to 0 in properties
Add draw method and give it a context parameter
Use drawImage canvas method on context with image and xy properties as arguments for now
Now we use import/export to link script.js with player.js
Export default(to export the whole file) on class Player
In script.js, import Player from "./player.js" (./ --> means from the same folder)
\*For named exports, you will need to wrap whatever you are importing with {}, for default exports, you don't
To test if the above export import works, create a player variable in the load event listener in script.js and create an instance of Player object with the required arguments(canvas width and height) using the new keyword
console.log player and inspect in the browser to check if the properties in Player class is shown. If it does, it works!
Remove the console.log and use draw method on player with the required argument
You should see the image of the sprite sheet you are using
Back to draw method in Player, remember it expects 3, 5, or 9 arguments
3 arguments allows you to move the sprite sheet base on the xy coordinate, starting from the top left
5 arguments means you are resizing the sprite sheet when from xy coordinate to the two sets of numerals you input as the last 2
9 arguments you are adding sx, sy, sw, sh at the front where you want to crop the image, and the original 4 dx, dy, dw, dh of where you want to place that image
Change dx, dy, dw, dh to the respective xy position, width and height
sx, sy will be 0 to crop the first sprite
sw, sh will be the width and height in Player properties because the values inside represent the exact dimensions of the sprite
For sx, multiplying width of the sprite with a number will visually show the sprite position on the row depending on the number
For sy, multiplying height with a number will show the position of the sprite on the column
Together, sx and sy can navigate the sprite sheet
Instead of hardcoding that number in sx and sy, we should use class properties to represent these numbers
Add frameX property and set it to 0 in Player to control x navigations
Add frameY property and set it to 0 to control y navigations
Now replace the hardcoded numbers in sx and sy with frameX and frameY
As you can see when changing the values in frameX and frameY, frameX will notably cycle through the sprite sheet for movement while frameY will be cycled when moving through different states
Let's place the player on the 'ground' which is on the bottom x axis of the canvas
To do this, deduct gameHeight(total canvas height) with height(sprite height) in y property
Move it to the middle of the x axis by deducting gameWidth divided by 2 with width divided by 2 in x property
Now, we will work on the inputs of the player
Create an InputHandler class in input.js and export it whole
In properties, add lastKey and give it an empty string
lastKey will hold the latest key presses of the player
Add a window event listener for keydown and use event(or e) as its callback function
console.log event or e and inspect in browser to look at the various properties it has
The property of key that you see when inspecting is the one we are looking for because we want to track key presses
In previous project (2d game), we used if else statements to handle key inputs
We should now use switch statements to get familiar with it
switch statements requires an expression to validate which is the property key in the event
Add the case "ArrowLeft" and assign lastKey to "PRESS left" for now
Now create another window event listener but for keyup, using the same expression for the switch but instead of PRESS you replace it with RELEASE
In essence, one will track presses the other releases of the key
console.log(e.key) right before the switch statement in keydown event listener
Import/export like you did for player.js, but now for input.js in script.js
Just like you did for player, create an input variable and use the new key word to instantiate InputHandler which will run the code inside the function
Now, inspect the browser and in console you should see all your key presses(if you console.logged e.key)
In this game, we are only concerned with the arrow keys
Remove the e.key console.log and add the right direction case for each event listener
Add a break statement for each cases
console.log input.lastKey in the load event listener to check if it works
It won't work if you did not bind the event listeners to the this keyword in the callback
To bind it, you can use built in bind javascript method OR ES6 arrow function in the callback
ES6 arrow function do not necessarily bind it but they inherit their parent scope, called lexical scoping
Create an animate function in load event listener and move the console.log inside, and call requestAnimationFrame(animate) to endlessly loop the function
Call animate after the animate function
The key presses and releases should be should be shown in console now
Moving on to utils.js, create a drawStatusText function to keep track of when game ends
You will need the parameters of context(to decide where in the canvas you want to display the text) and of input(tracking the last key value)
Set font on context to desired size and font family in drawStatusText
Use fillText(text, x, y) on context
For text you want the last input: input.lastKey
Export drawStatusText
Since its only 1 function, you can use named export method and import it to script.js accordingly with the {}
Call drawStatusText in animate with the required arguments
You should see text stacked on top of each other when pressing left and right arrow keys
Use clearRect method in animate to clear previous paints
This will clear player from the canvas as you are only drawing it once when outside the animate function
Move player.draw inside animate
Once you can see the text on last input and the player sprite, remove console.log input.lastKey in animate
//State design pattern next
Go to state.js and create a states object variable and export it
Fill states object with STANDING_LEFT and STANDING_RIGHT properties, and set them to 0 and 1 respectively
Create State class and pass state in the constructor's argument
Turn state into a class property
In state design pattern, each state is defined by a separate function
Create a child class StandingLeft using the extends keyword to the parent, State class
In StandingLeft class, the player argument is passed because the direction inputs only concern the player
Use super method in the constructor to gain access to its parent class
The argument passed into super will determine what 'state', the argument in which you passed in State class becomes
In this case, "STANDING LEFT" is passed into super, and State class will recognise the argument passed into super as the state of that particular child class
Now, convert player argument into class property in StandingLeft
Add an enter method, and handleInput method in StandingLeft
Copy and paste StandingLeft to create a second set and replace StandingLeft with StandingRight, and do the same for the super argument
\*4 principles of OOP --> Encapsulation, inheritance, polymorphism, Abstraction
Child classes will look inside itself first before moving to parent class if it cannot find properties, or calls inside itself. This is inheritance
Polymorphism is when identically named methods inside child classes exhibit different behaviors
handleInput method will use the input argument to determine which key represent which direction
The enter method will visually show in which frame the sprite is posing(standing left or right in this case)
In the sprite sheet, standing right pose is at index 0(1st column) while standing left is at index 1(2nd column)
You will access the frameY property in Player class to achieve the above so use this keyword to gain access to player's frameY property and set it to 1
Do the same for StandingRight but set it to 0 instead
In specific states, objects will only react to certain inputs defined explicitly and inputs that are not defined will be ignored
In handleInput for StandingLeft, if input equals to "PRESS right" is true(PRESS right can be found in input.js for ArrowRight), set state to StandingRight(comment the set state part because it's pseudo code and we cannot do it yet)
Do the same for StandingRight but for the opposite
To enable the above, you will need to add a method in Player class that will have state as its argument
Add setState method with state argument
Inside setState, set currentState property to states array with state argument as its array item
Since states is currently empty, setState will not work for now
After setting currentState to new state(which is empty for now), call enter method on currentState
Moving back to handleInput's pseudo code, now we have setState method to swap between states, remove the comment and use the setState method on player
Since setState in Player requires a state argument, use the properties that we have defined in the states object, STANDING_LEFT and STANDING_RIGHT
For StandingLeft child class, if input is right, we want the state of the player to turn right(STANDING_RIGHT)
Pass the relevant states property as the argument in the setState method in handleInput
Do the same for StandingRight class
Now, export the child classes and import into player.js so Player class can gain access to them
In the states object, STANDING_LEFT and RIGHT are what we call enum where the object properties are represented by numbers for easy access
In the states array property in Player, instead of an empty array, create new instance of StandingLeft and StandingRight in that order(the order needs to reflect the order in your enum)
We will also need the ability to call handleInput over and over to listen for new inputs and change states when needed
Create update method in Player with input argument
Call handleInput method with input argument on currentState which will determine what inputs handleInput is being processed in state.js
In animate, call update on player with the required argument
In the states array in Player, since StandingLeft and StandingRight class requires the player argument, you will need to pass it along in the corresponding StandingLeft and StandingRight instances in states array as well
Since the instances are created within the player argument(Player class), you can just use this keyword as their arguments
To be clearer, we can display currentState as text on the canvas
We will do this using the drawStatusText function
Pass the player argument to both drawStatusText method in animate and the original
Use fillText(text, x, y) method in drawStatusText in utils.js to display the text at your desired coordinates, using the currentState property in Player(you'll need to access state as well in state.js)
Change this.states[0] to this.states[1] because STANDING_RIGHT is your starting position
Once StandingLeft and StandingRight works, let's implement a sitting down position
Add ArrowDown cases in InputHandler
Create a new class in state.js called SittingLeft and copy the code that you have in StandingLeft or right with the appropriate changes for super argument and frameY position
For the handleInput method in SittingLeft, if right arrow key is pressed, setState to SITTING_RIGHT(which we have not created), else if up is pressed, setState to STANDING_LEFT(because player is facing left currently)
Now add SITTING_LEFT and RIGHT to the states object
Now, add an ArrowUp case in InputHandler
With SittingLeft done, do the same for SittingRight, changing areas where appropriate(super, frameY, conditions)
Import the new state classes into player.js
Create new instances of them in states array in Player just like StandingLeft and right
The logic for SittingLeft and right is done but we have no access to it currently
In StandingLeft and right conditions, add an else if for down key press and set them to the appropriate sitting direction
Test it out in the browser, player should be able to turn left and right standing and sitting down and stand back up
Maybe you want player to stand up immediately if the down key press is let go
Add another else if condition in handleInput for SittingLeft where if input is RELEASE down, setState to STANDING_LEFT
Do the same for SittingRight
This will revert player to the standing pose after down key press is released(if that is what you want for your game)
This is only to show you changes are easy to make for player behaviour with this code structure
If you implemented the RELEASE down condition, the PRESS up condition is not needed anymore since player goes into standing position after down key is released
Now, let's implement running left and right
Add running left and right to states object
Create RunningLeft and RunningRight child classes(just like the previous child classes) and make the appropriate changes
For the conditions, when right key is pressed in RunningLeft(the opposite direction,), setState to running right
Else if RELEASE left, setState to standing left
Else if down key press, setState to sitting left
Do the same for RunningRight with the appropriate directions
Just like sitting states, you need a way to gain access to the running states
Since player should run while in standing state, we should add an else if condition to allow player to enter running state in standing state
In StandingLeft, add an else if input is left key press, setState to running left
\*\*For some reason you do not need RELEASE left right condition to revert back to standing not sure why
Do the same for StandingRight with the appropriate changes
In player.js, import the newly created child classes and make new instances of these classes in the states array in Player(in the correct order according to your enum)
Once running states are shown correctly, we want to actually show the running animation(cycling through the running sprite row)
Add speed property in Player and set it to 0
Add maxSpeed property in Player and set it to 10;
In the enter methods for RunningLeft and right, set speed to negative maxSpeed for left and the opposite for right
In update method for Player, increment x position by speed
Set speed back to 0 in standing and sitting states in their enter methods to prevent player from moving indefinitely when left right keys are pressed
player sprite should now stop when you release left or right keys
To prevent player from moving outside of canvas, in Player update, create a horizontal movement section with the previous x increment by speed inside
Add a condition inside this section where if x is less than or equal to 0, set x to 0(for the left side)
Else if x is more than or equal to gameWidth minus width, set x to gameWidth minus width(for the right side)
We will now go into jumping and falling animation
Player will swap into falling state at peak height but will not require user input, unlike the other basic states
Create Jumping enums in states object
Just like before, create Jumping state classes and change the values in constructor and enter method accordingly
Delete speed in enter(if you copy pasted it) because you want jumping speed to be inherited(maybe)
Delete everything in handleInput method(again, if you copy pasted it from previous state templates)
Before we add conditions in handleInput, let's just add conditions for jumping states in the standing states(because player needs to be on the 'ground' before it can jump)
Import them into player.js and add the new instances into states array property
You can check your browser to see if the input for jump is correct
handleInput in the jumping states are empty which is why player is stuck in jump mode without any way to 'unstuck' it
We will now add helper properties in Player and add a vertical movement section in Player update
Add velocityY property in Player and set it to 0
Add weight property and give it 0.5 value
In vertical movement section, increment y by velocityY
Incrementing by velocityY will currently do nothing since the value is 0
Add a condition where if y is less than gameHeight minus height(meaning if player is off the ground/x axis), increment velocityY by weight
Else, set velocityY back to 0
In state.js, in the enter method for jumping states, decrement velocityY by 20
With the addition of jumping states and other states that require the jumping states, we will need to check if player is on the ground or off the ground
Add a helper method in player called onGround
We know that if player y position is less than gameHeight minus height, it is off the ground(written as the condition in vertical movement to check if player is on the ground)
In onGround method, we can just return the reverse of the above expression to get a true or false value(add equals to as well)
If it is true, player is on the ground and vice versa
Since you have this onGround helper method, you can use it in the condition in vertical movement section instead of writing the reverse conditions manually
Change the condition if y is less than gameHeight minus height to if onGround is not true(!onGround())
In jumping left state, if right key input is pressed, setState to jumping right
Do the same for jumping right with the appropriate directional key
In the browser, you will see that player will keep jumping when you press the left and right keys after pressing up
This is because in the enter methods, velocityY is being decremented(up direction in the y axis) for each left and key presses
To prevent this, we will add a condition where velocityY is decremented only when player is on the ground
Currently, player can only jump vertically with no changes in x position
Give player some horizontal movement by setting speed in enter methods for jumping states by half the maxSpeed(For jumping left state, speed must be negative as on the x axis, moving left is negative)
When testing the jumps, you will notice that player do not revert back to standing states
Add an else if condition in jumping states' handleInput, where if player is on the ground, setState back to the appropriate standing state
Notice you do not slide on the ground anymore as well because you are in standing state now
To understand the jumping states better, observe how velocityY is being incremented
As you know, upwards in the y axis is negative and downwards is positive
Since in the enter methods in jumping states you are decrementing velocityY by a value(20 in this case), velocityY starts at -20 and gets incremented by the weight, which is 0.5
Negative is upwards so starting point at -20 means player will 'jump' and -20 will be incremented by 0.5 till it reaches 0 which is the peak
Since positive in y axis is down, player will be fall and once player is on the ground, the else condition in Player update kicks in and velocityY is set to 0
In javascript canvas, if your weight has too high of a value, the browser could possibly not set velocityY back to 0 on time and player might clip through the ground(set weight to like >10 and see that player will clip through the ground)
Tom prevent this, set a condition in Player update where if y is more than gameHeight minus height(meaning if player ever clips the below the x axis), set y to gameHeight minus height(set player back on the ground)
We will now work on the falling states
Add falling states into states object
Create the falling states child classes with appropriate changes in super and frameY
In their handleInput methods, setState to their appropriate falling directions(\*\*Remember you are changing between states all the time with each different key press, if a key press does not change the state, then there is no need to change the state)
Imagine what the player is doing in that state and adjust the handleInput conditions logically(test it out in the browser)
Import them and instantiate them in states array in Player
So how do we enter falling state?
Imagine you are the player, you jump, reaches peak and that's where you start falling
So you want to add the falling states in the jumping states handleInput
Since we know when player starts falling, velocityY will be more than 0, we add a condition where if velocityY is more than 0, setState to falling state with the appropriate direction in the jumping states
Since there are falling states now, we can remove the else if statement to check if player is on the ground in jumping states because falling states check for them now
Once the states are working correctly, we will start animating the sprite instead of just showing frame 0 of the sprite position
In Player properties, add the initial starting maxFrame and set it to 6(the max number of standing sprite which starts from 0 because javascript)
In Player draw, add the condition where if frameX is less than maxFrame, increment frameX
Else set frameX back to 0
In state.js, when entering different states, set maxFrame according to the max frames of the state(i.e standing left has 6, standing right has 6, sitting left has however many etc)
To cap animation frames from going too fast, we can cap it via time stamps property which requestAnimationFrame method will provide(as seen in previous projects)
In load event listener, add a holder variable called lastTime and set it to 0
Pass timeStamp into animate as a parameter
In animate method, add deltaTime which represents the difference between the this animation loop(timeStamp) and last animation loop(lastTime)
After the above, set lastTime to timeStamp so the next loop will update lastTime value to be reused in the deltaTime calculation
Pass initial value of 0 when calling animate because the first requestAnimationFrame is undefined since there is nothing to animate yet so you need to give it a starting value
deltaTime will be used to unify and standardized the speed of changing frames horizontally across the sprite sheet on all machine types
This will happen on the draw method so pass deltaTime as a required parameter in animate where draw is called on player
Do it in the original draw method in Player as well
We will need three helper properties in Player to use deltaTime
Add fps property and set it to 30
Add frameTimer and set it to 0
Add frameInterval and set it to 1000 divided by fps(which is the number of milliseconds we want passed before displaying the next frame of the sprite sheet)
In Player draw, only when frameTimer is more than frameInterval, you invoke the condition for incrementing frameX you have written before(move that condition into the new condition you made)
Also set frameTimer back to 0 in the new condition after invoking the frameX condition
Else, increment frameTimer by deltaTime to accumulate the milliseconds before drawing next frame(because how else will frameTimer ever be more than frameInterval?)
Your screen's refresh rate is the bottleneck regardless of how high you increase fps property value so the animation frame will always be capped at the maximum refresh rate your screen can handle(i.e using virtual machine on my pc changing fps to 200, 150 or even 60 shows the same animation speed. When you change it to 50 or below only then you can see animation starts to slow down meaning my refresh rate cap is around 50)
Done!(Still confused but whatever)
