# Sumopixel

### Code Editor Docs

The idea is that you can write JavaScript as you like and access the same features of the Pixel that are being used by the editor itself. So we have exposed some of the variables and functions into window scope, which means you can access them in the code window.

# Variables
Here are the variables available in the code window

`gridSize` - If grid size is 32 x 32 then this value is `32` etc.
`brushColor` - Color code of the currently selected color, eg `#ffffff`
`scene` - Grid data array of the current frame
`frames` - total number of frames (counted starting from zero)
`currentFrame` - number of the current frame (starts from zero)

# Methods

### play
`play(fps)`
Toggles play / pause. Parameter `fps` is optional frames-per-seconds if you want to set the animation speed.
For example: `play(15)` would start animation playback at 15 frames per second.

### shiftUp, shiftLeft, shiftRight, shiftUp
- `shiftUp()` - Moves grid content one row upwards
- `shiftLeft()` - Moves grid content one column left
- `shiftRight()` - Moves grid content one column right
- `shiftDown()` - Moves grid content one row downwards

### animate
`animate(index, all)`
Animates the grid with ripple effect.

- `index` is pixel number of the center point where `0` is the first pixel and length of the grid is the last pixel.
- `all` is optional parameter to tell if all pixels should be animated. Without all parameter only empty pixels are animated.

### notify
`notify({ type, message })` - Triggers notification.

For example.
Following triggers success notification
```notify({ type: 'success', message: 'Done!' })```

Following triggers error notification
```notify({ type: 'error', message: 'Something went wrong..' })```

### updateGrid

`updateGrid({ index, props })`
Re-renders the grid with new data

For example, following would make 10th pixel in the grid black.
```
    updateGrid({
		index: 10, 
		props: {
			color: '#000000'
		}
	})
```

### setBrushColor

`setBrushColor(colorCode)` - Set brush color. ColorCode is any HTML standard color code, such as `#ffffff`, `white` or `rgb(255, 255, 255)` etc.

### paintPixel

`paintPixel(index)` - paints given pixel with current brush color

### erasePixel

`erasePixel(index)` - Erases given pixel and makes it transparent

### gotoFrame

`gotoFrame(frame)` - Goes into frame. Same as clicking on any frame button.

### addFrame

`addFrame()` creates new frame. Same as clicking plus button to add frame.

# Example 1: Fill grid with random colors

We determine how many pixels there are in the grid from gridSize. Then loop through each pixels and use `updateGrid()` to set the color.

```
const allPixels = gridSize * gridSize

function random(max) {
  return Math.floor(Math.random() * Math.floor(max))
}

const randomColor = () => `rgb(${random(255)}, ${random(255)}, ${random(255)})`

for (let px = 0; px < allPixels; px++) {
  updateGrid({
    index: px,
    props: {
      color: randomColor()
     }
  })
}
```

# Example 2: generate frames

This example generates 10 frames

```
async function createFrames() {
	for (let i = 0; i < 10; i++) {
		await addFrame()
			.then(() => paintPixel(random(gridSize * gridSize)))
	}
}

createFrames()
```

# Example 3: Roll it up

This moves all pixels one row up in every 100ms.

```
let timer = setInterval(() => {
   shiftUp()
}, 100)
```

# Example 4: Listening to keys

We can use standard JavaScript event listeners to catch key strokes and map them with functionality.

```
window.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowUp') {
    shiftUp()
  }
  if (e.key === 'ArrowDown') {
    shiftDown()
  }
  if (e.key === 'ArrowLeft') {
    shiftLeft()
  }
  if (e.key === 'ArrowRight') {
    shiftRight()
  }
})
```

# Example 5: Invert colors

Go through the data and invert each pixel color

```
const allPixels = gridSize * gridSize

function padZero(str, len) {
    len = len || 2;
    var zeros = new Array(len).join('0');
    return (zeros + str).slice(-len);
}

function invertColor(rgb) {
  if (rgb == null) {
    return 'rgb(255, 255, 255) '
  } else {
    if (rgb.indexOf('#') === 0) {
      // hex color
      rgb = rgb.replace('#', '')
      if (rgb.length === 3) {
        rgb = rgb[0] + rgb[0] + rgb[1] + rgb[1] + rgb[2] + rgb[2];
      }
      let r = parseInt(rgb.slice(0, 2), 16)
      let g = parseInt(rgb.slice(2, 4), 16)
      let b = parseInt(rgb.slice(4, 6), 16)
      r = (255 - r).toString(16);
      g = (255 - g).toString(16);
      b = (255 - b).toString(16);
      return `#${padZero(r)}${padZero(g)}${padZero(b)}`
    } else {
      // rgb color
      let rgbArr = rgb.split(',')
      let r = rgbArr[0].replace('rgba(', '').replace('rgb(', '')
      let g = rgbArr[1]
      let b = rgbArr[2].replace(')', '')
      r = (255 - r).toString();
      g = (255 - g).toString();
      b = (255 - b).toString();
      return `rgb(${r}, ${g}, ${b})`
    }
  }
}

for (let px = 0; px < allPixels; px++) {
  updateGrid({
    index: px,
    props: {
      color: invertColor(scene[px].color)
     }
  })
}

```

# Example 6 - Cycle through frames

Go to next frame in every 500ms.

```
let frame = 0

setInterval(() => {
   if (frame >= frames.length - 1) {
     frame = 0
   } else {
     frame++
   }
   gotoFrame(frame)
}, 500)
```
