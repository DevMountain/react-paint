# react-paint

<img src="https://devmounta.in/img/logowhiteblue.png" width="250" align="right">

# Project Summary

In this project, we will create a paint app that will allow us to practice passing data from a parent component to a child component via props, and update a parent component's state from a child component.

## Setup

1.  `create-react-app react-paint`.
2.  `cd` into the project directory.
3.  Run `npm install`.
4.  Remove service worker from `src/index.js`
5.  Run `npm start`.
6.  In a seperate terminal, `cd` into the project directory.

## Step 1

### Summary

In this step, we will create the basic structure for the components we will be using.

### Instructions

* Create a components folder inside of src.
* Inside the components folder, create the following component files:
    * PaintCanvas.js
    * ColorPicker.js
    * Square.js
* Create a class component for Square that renders a `div` with these styles `{ height: 10, width: 10, border: '1px solid black }`
* Create a stateless functional component from ColorPicker that, for now, will render a div with 4 buttons that say 'blue', 'yellow', 'green', 'purple'.
* Create a class component for PaintCanvas that will render both the ColorPicker component and the Square component.
* In `src/App.js`, render the PaintCanvas component.

### Solution

<details>

<summary> <code> ./src/App.js </code> </summary>

```js
import React, { Component } from 'react';

import PaintCanvas from './components/PaintCanvas'

class App extends Component {
  render() {
    return (
      <div>
        <PaintCanvas />
      </div>
    );
  }
}

export default App;
}
```

</details>

<details>

<summary> <code> ./src/components/PaintCanvas.js </code> </summary>

```js
import React, { Component } from 'react'
import ColorPicker from './ColorPicker'
import Square from './Square'

export default class PaintCanvas extends Component {

  render() {
    return (
      <div>
        <ColorPicker />
        <Square />
      </div>
    )
  }
}
```

</details>

<details>

<summary> <code> ./src/components/ColorPicker.js </code> </summary>

```js
import React from 'react'

export default function ColorPicker(props) {
  return (
    <div>
      <button>blue</button>
      <button>yellow</button>
      <button>green</button>
      <button>purple</button>
    </div>
  )
}
```

</details>

<details>

<summary> <code> ./src/components/Square.js </code> </summary>

```js
import React, { Component } from 'react';

export default class Square extends Component {

  render() {
    return (
      <div style={{
        height: 10, 
        width: 10, 
        border: '1px solid black'
      }}></div>
    )
  }
}
```

</details>

## Step 2

### Summary

In this step, we will be using state to keep track of the current selected color.

### Instructions

* Add `constructor` method to PaintCanvas.
    * method should call super()
    * create initial state with the following properties:
          - selectedColor: 'blue'
* Create a method to update the selected color on the component's state.  Name the method `changeSelectedColor`.  The method will take in a color as a parameter and use `this.setState` to update the `selectedColor` property on state.  Rememer to bind `this`!

### Solution

<details>

<summary> <code> ./src/components/PaintCnavas.js </code> </summary>

```js
import React, { Component } from 'react'
import ColorPicker from './ColorPicker'
import Square from './Square'

export default class PaintCanvas extends Component {
  constructor() {
    super()

    this.state = {
      selectedColor: 'blue'
    }

    this.changeSelectedColor = this.changeSelectedColor.bind(this)
  }

  changeSelectedColor(color) {
    this.setState({
      selectedColor: color
    })
  }

  render() {
    return (
      <div>
        <ColorPicker />
        <Square />
      </div>
    )
  }
}
```

</details>

## Step 3

### Summary

In this step, we will make it so when we click on a color in `ColorPicker`, it will updated the `selectedColor` property on `PaintCanvas` state.

### Instructions

* In PaintCanvas.js, pass the `changeSelectedColor` method as a prop on `ColorPicker`.  Name the prop `handleColorClick`.
* In ColorPicker.js, add an `onClick` event listener to each of the buttons, that takes a callback function that will invoke the handleColorClick method from the props object, and pass in the color of that button. 

### Solution

<details>

<summary> <code> ./src/components/PaintCanvas.js </code> </summary>

```js
import React, { Component } from 'react'
import ColorPicker from './ColorPicker'
import Square from './Square'

export default class PaintCanvas extends Component {
  constructor() {
    super()

    this.state = {
      selectedColor: 'blue'
    }

    this.changeSelectedColor = this.changeSelectedColor.bind(this)
  }

  changeSelectedColor(color) {
    this.setState({
      selectedColor: color
    })
  }

  render() {
    return (
      <div>
        <ColorPicker handleColorClick={this.changeSelectedColor}/>
        <Square />
      </div>
    )
  }
}
```

</details>

<details>

<summary> <code> ./src/components/ColorPicker.js </code> </summary>

```js
import React from 'react'

export default function ColorPicker(props) {
  return (
    <div>
      Color Picker
      <button onClick={() => props.handleColorClick('blue')}>blue</button>
      <button onClick={() => props.handleColorClick('yellow')}>yellow</button>
      <button onClick={() => props.handleColorClick('green')}>green</button>
      <button onClick={() => props.handleColorClick('purple')}>purple</button>
    </div>
  )
}
```

</details>

## Step 4

### Summary

Awesome!  We can now click on a color, and it updates the PaintCanvas component's state.  Now we can pass this information along to the square, so when we click on it, we can change its background color the color that was selected!

### Instructions

* In PaintCanvas.js, pass the selectedColor to the `Square` component via props.  name the prop `selectedColor`.
* Now, in Square.js, we need to keep track of the background color on the `Sqaure` component's state.
   * Add the `constructor` method to the Square class.  invoke `super`, and create the state object with a property `backgroundColor`.  set the default value for `backgroundColor` to `white`.
* lets use the background color to style the div.  Add the following to the style object on the div in the render method: `background: this.state.backgroundColor`.
* Now we need to add an `onClick` event listener on the `div` so we can change the background color to the color that was passed to us via props.  
   * Add a method to the `Sqaure` class called `changeBackgroundColor`.  This method should update the `backgroundColor` property on state to the selected color on props. Make sure to bind `this`.
   * Add an `onClick` to the `div` in the render method, and give it the `changeBackgroundColor` method as the callback that will be invoked when we click on the div.

### Solution

<details>

<summary> <code> ./src/components/PaintCanvas.js </code> </summary>

```js
import React, { Component } from 'react'
import ColorPicker from './ColorPicker'
import Square from './Square'

export default class PaintCanvas extends Component {
  constructor() {
    super()

    this.state = {
      selectedColor: 'blue'
    }

    this.changeSelectedColor = this.changeSelectedColor.bind(this)
  }

  changeSelectedColor(color) {
    this.setState({
      selectedColor: color
    })
  }

  render() {
    return (
      <div>
        <ColorPicker handleColorClick={this.changeSelectedColor}/>
        <Square selectedColor={this.state.selectedColor}/>
      </div>
    )
  }
}
```

</details>

<details>

<summary> <code> ./src/components/Square.js </code> </summary>

```js
import React, { Component } from 'react';

export default class Square extends Component {
  constructor() {
    super()

    this.state = {
      backgroundColor: 'white'
    }

    this.changeBackgroundColor = this.changeBackgroundColor.bind(this)
  }
  changeBackgroundColor() {
    this.setState({
      backgroundColor: this.props.selectedColor
    })
  }

  render() {
    return (
      <div style={{
        height: 10, 
        width: 10, 
        border: '1px solid black',
        background: this.state.backgroundColor
      }} onClick={this.changeBackgroundColor}></div>
    )
  }
}
```

</details>

## Step 5

### Summary

Awesome!  We can now click on a color in the color picker, and then click on the square, and it changes colors.  SO COOL!  But with only one square, this paint app is super boring.  Since we know we can reuse a component, why don't we just add 4,999 more squares so we have a total of 5,000 squares!

### Instructions

* In PaintCanvas.js add a method called `draw`.
   * this is the logic that should go inside of `draw`:
   ```
   let squares = []
    for (var i = 0; i < 5000; i++) {
      squares.push(<Square selectedColor={this.state.selectedColor} />)
    }

    return squares
    ```
* In the render method replace `<Square selectedColor={this.state.selectedColor} />` with 
```
<div style={{display: 'flex', flexWrap: 'wrap'}}>
   {this.draw()}
</div>
```

### Solution

<details>

<summary> <code> ./src/components/PaintCanvas.js </code> </summary>

```js
import React, { Component } from 'react'
import ColorPicker from './ColorPicker'
import Square from './Square'

export default class PaintCanvas extends Component {
  constructor() {
    super()

    this.state = {
      selectedColor: 'blue'
    }

    this.changeSelectedColor = this.changeSelectedColor.bind(this)
  }

  changeSelectedColor(color) {
    this.setState({
      selectedColor: color
    })
  }

  draw() {
    let squares = []
    for (var i = 0; i < 5000; i++) {
      squares.push(<Square selectedColor={this.state.selectedColor} />)
    }

    return squares
  }

  render() {
    return (
      <div>
        <ColorPicker handleColorClick={this.changeSelectedColor}/>
        <div style={{display: 'flex', flexWrap: 'wrap'}}>
         {this.draw()}
        </div>
      </div>
    )
  }
}
```

</details>

## Black Diamond

Make it so when you click down on the mouse, it will change the color of all of the squares that the mouse enters until you release the mouse.  A continuous painting motion.

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

© DevMountain LLC, 2018. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

<p align="center">
<img src="https://devmounta.in/img/logowhiteblue.png" width="250">
</p>
