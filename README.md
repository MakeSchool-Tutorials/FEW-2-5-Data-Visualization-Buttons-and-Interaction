# FEW 2.5 Data Visualization Interaction tutorial

Handling user interaction is the stock and trade of front end developers. Applying interaction to a visualization can add a lot of engagement and provide new insights into the data. 

## Why should you learn this? 

This tutorial will cover a few standard interactions that appear in many projects core to all desktop and mobile software UI. 

## Objectives 

1. Create IU systems that handle one or more choices
1. Use closures 
1. Use callbacks to handle interaction
1. Use CSS `transition` to create motion

## Buttons, Toggle Buttons, and Button Groups 

A common UI requirement is a button. A button handles clicks or taps. 

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
    // Rings bell
}) 
```

### Toggle Buttons 

In some cases, you'll want a button that toggles between two states each time it is tapped. Think of this as a light switch. 

You might make a toggle button like this: 

```HTML
<button id="toggle-button">Mode</button>
```

Think of a toggle button like a light switch. The first time you interact it turns the light on, the next time it turns the light off. The code behind a toggle button is more complex. 

First, the state of the button needs to be expressed in the style of the button. In other words, when we look at a toggle button we need to know what state it's in. 

Second, the button needs to execute different code depending on its state. When alight is in the off state interaction turns the light on. 

You migth set up a toggle button like this: 

```JavaScript 
// Get a reference to the button
const toggleButton = document.getElementById('toggle-button')

// Define a variable to hold the state
let buttonState = false

// Define a function to handle the appearance
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

// Set up the initial appearance
selectButton(toggleButton, buttonState)

// Define a function to handle clicks
toggleButton.addEventListener('click', (e) => {
    buttonState = !buttonState // change the state
    selectButton(toggleButton, buttonState) // change the appearnace
    // do some other code...
})
```

The code above is a good start it would be difficult to use if you had lots of toggle buttons. 

Consider this: 

```JavaScript
// A funtion takes a button and a callback
// The callback receives the button state as an argument
function makeToggleButton(button, callback) {
    let buttonState = false
    button.addEventListener('click', (e) => {
        buttonState = !buttonState
        selectButton(button, buttonState)
        callback(buttonState)
    })
    selectButton(button, buttonState)
}
// Callback for the toogle button
makeToggleButton(someButton, (state) => {
    if (state) {
        // do something when state is true
    } else {
        // do something when state is false
    }
})
```

In this snippet, the `button`, `callback`, and `buttonState` are encapsulated in the `makeToggleButton()` function and captured by the function in the event listener via 'closure'. 

This function takes a button element and a callback. It adds an event listener to the button that handles changes to the appearance of the button based on its state. This event listener also calls the callback function and passes the state of the button. 

In the last code block, you'll see we're passing a button element and the callback. This callback handles what happens when the button is toggled. 

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

An easy approach is to handle a click on any of the buttons in the group by looping through the group, 'deselecting' all of the buttons, and 'selecting' the button that was clicked. 

To get a collection of buttons to give all of your buttons the same class name. 

```HTML
<div>
    <button class="random-button-group" data-name="a">A</button>
    <button class="random-button-group" data-name="b">B</button>
    <button class="random-button-group" data-name="c">C</button>
</div>
```

Here all buttons have the class name: `random-button-group`. 

```JavaScript 
// This function takes a collection of elements and a callback
// The callback receives the button that was clicked as an argument
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

Here I used a function that takes a collection of button elements, and a callback. It loops through all of the buttons in the collection and adds an event listener to each. The handler function for the event listener captures `buttons`, `button`, and `callback` variables via closure. 

When a button is clicked the 'click' handler loops through the collection and compares each button from the collection with variable `button` which is the button that was clicked. If they match that button is selected, otherwise the button is deselected. 

Last it calls the callback function and passes the currently selected button. 

To handle the callback realize that all of the buttons in the group will call the same callback, and the callback will receive the button that was clicked as a parameter. 

At this point, you'll need some unique way for each button in a group to handle the callback. Keep in mind there is only one callback, clicking any button in the group calls the same callback.

To assign unqiue data to each button element use a data attribute. 

`<button class="random-button-group" data-name="a">A</button>`

Here the data attribute is `data-name`. These attributes are for developer specified information and always begin with `data-` followed by any name you decide. Like all attributes, the value must be quoted. 

To access a data attribute with JS use the `dataset` property. 

`el.dataset.name`

Using the `makeGroup()` function above you might do this:

`const buttons = document.querySelectorAll('.random-button-group')`

Get a collection of buttons by class name. This gets the buttons from the example. 

Then set up the group. 

```JavaScript
makeGroup(buttons, (button) => {
    buttons.forEach((el, i) => {
        const h = data[i][button.dataset.name] * 400
        el.style.top = `${400 - h}px`
        el.style.height = `${h}px`
    }) 
})
```

Here a collection of buttons is passed in, along with a callback that takes `button` as a parameter. The callback looks at each button in the collection and sets some properties based on the `data-name` property of the button that was clicked. 

## Challenges 

The code above is started in the direction of making a reusable system. It leaves a few things out. 

- Name Space - The code above 'pollutes` global space with several function names. Better to put all of these under a single namespace. 
    - Challenge: Define a single global object that owns all of these functions. 
- The styles are fixed - The styles for the selected and non-selected states are fixed.
    - Challenge: Allow your system to set the style for buttons. An easy approach might be to use a class name, though this would require that those classes be defined somewhere. 
