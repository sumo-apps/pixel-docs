# Sumopixel

### Code Editor Docs

The idea is that you can write JavaScript as you like and access the same features of the Pixel that are being used by the editor itself. So we have exposed some of the variables and functions into window scope, which means you can access them in the code window.

# Pixel object
Here are the variables available in the code window

## Vars

- `Pixel.gridSize` - If grid size is 32 x 32 then this value is `32` etc.
- `Pixel.brushColor` - Color code of the currently selected color, eg `#ffffff`
- `Pixel.frames` - total number of frames (counted starting from zero)
- `Pixel.currentFrame` - number of the current frame (starts from zero)

## Methods

### setPixel
`Pixel.setPixel(x, y, color)`

Sets pixel color of given x, y coordinate. 
For example `Pixel.setPixel(10, 10, 'white')`

### setPixelByIndex
`Pixel.setPixelByIndex(index, color)`

Sets pixel color at given index (0 - total amount of pixels). 
For example `Pixel.setPixelByIndex(64, '#cc00ff')`

### play
`Pixel.play(fps)`

Toggles play / pause. Parameter `fps` is optional frames-per-seconds if you want to set the animation speed.
For example: `play(15)` would start animation playback at 15 frames per second.

### gotoFrame

`Pixel.gotoFrame(frame)` - Goes into frame.

### addFrame

`Pixel.addFrame()` creates new empty frame.

### addFrame

`Pixel.copyFrame()` creates new frame as a copy from the current frame.

### wait

`Pixel.wait(seconds)` waits given time in seconds and returns promise.
For example: `Pixel.wait(3).then(() => { do something })`


### shiftUp, shiftDown, shiftLeft, shiftRight

`Pixel.shiftLeft()` moves everything left by one pixel.

# Other

### animate
`animate(x, y)`
Animates the grid with ripple effect.

### notify
`notify({ type, message })` - Triggers notification.

For example.

Following triggers success notification
```notify({ type: 'success', message: 'Done!' })```

Following triggers error notification
```notify({ type: 'error', message: 'Something went wrong..' })```

# Tips

If you don't want to type Pixel. front of all methods and vars you can do this:

```
const { addFrame, play } = Pixel

addFrame()
play()
```

It would be as same this
```
Pixel.addFrame()
Pixel.play()
```

# Example 1: Fill grid with random colors

We determine how many pixels there are in the grid from gridSize. Then loop through each pixels and use `updateGrid()` to set the color.

```
const { gridSize, setPixelByIndex } = Pixel

function random(max) {
  return Math.floor(Math.random() * Math.floor(max))
}

const randomColor = () => `rgb(${random(255)}, ${random(255)}, ${random(255)})`

for (let px = 0; px < gridSize; px++) {
  setPixelByIndex(px, randomColor())
}
```

# Example 2: generate frames

This example generates three frames with some colored lines and then starts playback

```
const { gotoFrame, addFrame, gridSize, setPixel, play } = Pixel

gotoFrame(0)

for (var x = 1; x <= gridSize; x++) {
  setPixel(x, 1, 'green')
}

addFrame()
gotoFrame(1)

for (var x = 1; x <= gridSize; x++) {
  setPixel(x, 2, 'red')
}

addFrame()
gotoFrame(2)

for (var x = 1; x <= gridSize; x++) {
  setPixel(x, 3, 'blue')
}

play(5)

```

# Example 3: Roll it up

This moves all pixels one row up in every 100ms.

```
let timer = setInterval(() => {
  Pixel.shiftUp()
}, 100)
```

# Example 4: Listening to keys

We can use standard JavaScript event listeners to catch key strokes and map them with functionality.

```
const { clearFrame, gridSize, setPixel, setPixelByIndex, gridColumns } = Pixel

let index = (gridSize / 2) + (gridColumns / 2)

setPixelByIndex(index, 'white')

window.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowUp') {
    clearFrame()
    index = index - gridColumns
    setPixelByIndex(index, 'white')
  }

  if (e.key === 'ArrowDown') {
    clearFrame()
    index = index + gridColumns
    setPixelByIndex(index, 'white')
  }

  if (e.key === 'ArrowLeft') {
    clearFrame()
    index = index - 1
    setPixelByIndex(index, 'white')
  }

  if (e.key === 'ArrowRight') {
    clearFrame()
    index = index + 1
    setPixelByIndex(index, 'white')
  }
})
```

# Example 5: Invert colors

Go through the data and invert each pixel color

```
const { gridSize, setPixelByIndex, frames, currentFrame } = Pixel

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

const scene = frames[currentFrame]

for (let px = 0; px < gridSize; px++) {
  setPixelByIndex(px, invertColor(scene[px].color))
}
```

# Example 6 - Cycle through frames

Go to next frame in every 500ms.

```
const { frames, gotoFrame } = Pixel

let frame = 0

setInterval(() => {
  if (frame < frames.length - 1) {
    gotoFrame(frame)
    frame++
  } else {
    frame = 0
  }
  gotoFrame(frame)
}, 500)
```
