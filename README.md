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
