# FEW 2.5 Data Visualization Interaction tutorial

Handling user interaction is the stock and trade of front end devlopers. Applying interaction to a visualization can add a lot of engagement and provide new insights into the data. 

## Why should you learn this? 

This tutorial will cover a few the standard interactions that appear in many projects core to all desktop and mobile software UI. 

## Objectives 

1. Create IU systems that handle one or more choices
1. Use closeures 
1. Use callbacks

## Buttons, Toggle Buttons and Button Groups 

A common UI requirement is the button. A button handles clicks or taps. 

### Buttons

Consider a button to run the same code every time it is clicked or tapped. It's like your doorbell. 

A button might be created like this: 

```HTML
<button id="hello-button">Login</button>
```

You might handle interaction with this button like this: 

```JavaScript
const helloButton = document.getElementById('hello-button')

helloButton.addEventListener('click', (e) => {
	// do stuff
}) 
```

### Toggle Buttons 

In some cases you'll want a button that toggles between two states each time it is tapped. Think of this like a light switch. 

You might make a toggle button like this: 

```HTML
<button id="toggle-button">Mode</button>
```

The code behind a toggle button is more complex. First, the state of the button needs to be expressed in the style of the button. Second, the button needs to execute different code depending on it's state. 

You set up a toggle button like this: 

```JavaScript 
const toggleButton = document.getElementById('toggle-button')

let buttonState = false

function selectButton(button, state) {
	button.style.border = '1px solid'
	button.style.borderRadius = '5px'
	button.style.margin = '5px'
	button.style.padding = '5px 10px'

	if (state) {
		button.style.backgroundColor = '#000'
		button.style.color = '#fff'
	} else {
		button.style.backgroundColor = '#fff'
		button.style.color = '#000'
	}
}

selectButton(toggleButton, buttonState)

toggleButton.addEventListener('click', (e) => {
	buttonState = !buttonState 
	selectButton(toggleButton, buttonState)
})
```

The code above is a good start it would be difficult to use if you had lots of toggle buttons. 

Consider this: 

```JavaScript
function makeToggleButton(button, callback) {
	let buttonState = false
	button.addEventListener('click', (e) => {
		buttonState = !buttonState
		selectButton(button, buttonState)
		callback(buttonState)
	})
	selectButton(button, buttonState)
}

makeToggleButton(someButton, (state) => {
	if (state) {
		// do something when state is true
	} else {
		// do something when state is false
	}
})
```

In this snippet the `button`, `callback`, and `buttonState` are encapsulated in the `makeToggleButton()` function and captured by the function in the event listener via 'closure'. 

This function takes a button element and a callback. It adds an event listener to the button that handles changes to the appearance of the button based on it's state. This event listener also calls the callback function and passes the state of the button. 

In the last code block you'll see we're passing a button element and the callback. This callback handles what happens when the button is toggled. 

You can add another button with: 

```JavaScript
makeToggleButton(lightSwitch, (state) => {
	if (state) {
		lightOn()
	} else {
		lightOff()
	}
})
```

### Button Groups 

The goal here to create a group of buttons that work together. Essentially a toggling group where only one button is active at a time. 

Use this when you need to chose one of two or more options. For example, showing only passengers by class, or embarkation. 

To make a group of buttons you'll need to have a collection of button elements. This will allow you to loop through the group and set the state of any or all buttons. 

An easy approach is to handle a click on any of the buttons in the group by looping through the group, 'deselecting' all of the buttons, and 'selecting' the button that clicked. 

To get a collection of buttons give all of your buttons the same class name. 

```HTML
<div>
	<button class="random-button-group" data-name="a">A</button>
	<button class="random-button-group" data-name="b">B</button>
	<button class="random-button-group" data-name="c">C</button>
</div>
```

Here all buttons have the class name: `random-button-group`. 

```JavaScript 
function makeGroup(buttons, callback) {
	buttons.forEach((button) => {
		selectButton(button, false)
		button.addEventListener('click', (e) => {
			buttons.forEach((b) => {
				if (b === button) {
					selectButton(b, true)
				} else {
					selectButton(b, false)
				}
			})
			callback(button)
		})
	})
}
```

Here I used a function that takes a collection of button elements, and a callback. It loops thropugh all of the buttons in the collection and adds an event listener to each. The handler function for the event listener captures `buttons`, `button`, and `callback` variables via closure. 

When a button is clicked the handler loops through the collection and compares each button from the collection with `button` which is the button that was clicked. If they match that button is selected, otherwise the button is deselected. 

Last it calls the callback function and passes the currently selected button. 

To handle the callback realize that all of the buttons in the group will call the same callback, and the callback will receive the button that was clicked as a parameter. 

At this point you'll need some unique way for each button in a group to handle the callback. Keep in mind there is only one callback, clicking any button in the group calls the same callback, and callback receives the button as a parameter. 

To assign unqiue data to each button element use a data attribute. 

`<button class="random-button-group" data-name="a">A</button>`

Here the data attaribute is `data-name`. These attributes are for developer specified information and aalways begin with `data-` followed by any name you decide. Like all attributes the value must be quoted. 

To access a data attribute with JS use the `dataset` property. 

`el.dataset.name`

Using the `makeGroup()` function above you might do this:

`const buttons = document.querySelectorAll('.random-button-group')`

Get a collection of buttons by class name. This gets the buttons from the example. 

Then set up the group. 

```JavaScript
makeGroup(buttons, (button) => {
	let i = 0
	elements.forEach((el) => {
		const h = data[i][button.dataset.name] * 400
		el.style.top = `${400 - h}px`
		el.style.height = `${h}px`
		i += 1
	}) 
})
```






