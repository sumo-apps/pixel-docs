# Sumopixel
### Code Editor Docs

The idea is that you can write JavaScript as you like and access the same features of the Pixel that are being used by the editor itself. So we have exposed some of the variables and functions into window scope, which means you can access them in the code window.

# Pixel object
Here are the variables available in the code window

## Vars

- `gridSize` - If grid size is 32 x 32 then this value is `1024` etc.
- `gridColumns` - Number of columns (or rows) in the grid.
- `brushColor` - Color code of the currently selected color, eg `rgb(255, 255, 255)`
- `frames` - total number of frames (counted starting from zero)
- `currentFrame` - number of the current frame (starts from zero)

## Methods

### setPixel
`setPixel(x, y, color)`

Sets pixel color of given x, y coordinate.\
For example `setPixel(10, 10, 'rgb(255, 255, 255)')`

### setPixelByIndex
`setPixelByIndex(index, color)`

Sets pixel color at given index (0 - total amount of pixels).\
For example `setPixelByIndex(64, 'rgb(255, 255, 255)')`

### listColors
`listColors()`
Outputs list of all color codes used in the scene into developer console.\

### invertColor
`invertColors(target)`

target is optional argument for telling which colors should be inverted.\
For example:
- `invertColors()` inverts all colors
- `invertColors('rgb(255, 255, 255)`) inverts only white colors
- `invertColors(['rgb(0, 0, 0,)', 'rgb(255, 255, 255)'])` inverts black and white colors.\
Hint: when you hover pixel with mouse you will get its color code as a tooltip

### replaceColor
`replaceColor(oldColor, newColor)`\
Replaces each pixel with given color with new color.

For example: `replaceColor('rgb(255, 255, 255)', 'rgb(183, 28, 28)')` turns all white pixels into red.

### drawHLine
`drawHLine(row, color)`\
Draws horizontal line at given row number (y-coordinate).

For example: `drawHLine(4, 'rgb(255, 255, 255)')` draws white line on fourth row

### drawVLine
`drawVLine(col, color)`\
Draws horizontal line at given column number (x-coordinate).

For example: `drawHLine(4, 'rgb(255, 255, 255)')` draws white line on fourth row

### play
`play(fps)`

Toggles play / pause. Parameter `fps` is optional frames-per-seconds if you want to set the animation speed.\
For example: `play(15)` would start animation playback at 15 frames per second.

### gotoFrame

`gotoFrame(frame)` - Goes into frame.

### addFrame

`addFrame()` creates new empty frame.

### copyFrame

`copyFrame()` creates new frame as a copy from the current frame.

### wait

`wait(seconds)` waits given time in seconds and returns promise.\
For example: `wait(3).then(() => { do something })`


### shiftUp, shiftDown, shiftLeft, shiftRight

`shiftLeft()` moves everything left by one pixel.

### clear
`clear(x, y)`\
Removes pixel from given coordinate.\
For example: `clear (1, 1)`

### clearFrame
`clearFrame()`\
Clears entire frame.

### clearAll
`clearAll()`\
Clears all frames.

# Other

### animate
`animate(x, y)`\
Animates the grid with ripple effect.

### notify
`notify({ type, message })` - Triggers notification.

For example.

Following triggers success notification\
```notify({ type: 'success', message: 'Done!' })```

Following triggers error notification\
```notify({ type: 'error', message: 'Something went wrong..' })```


# Example 1: Fill grid with random colors

We determine how many pixels there are in the grid from gridSize. Then loop through each pixels and use `updateGrid()` to set the color.

```
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
gotoFrame(0)

drawHLine(1, 'red')

addFrame()
gotoFrame(1)

drawHLine(2, 'green')


addFrame()
gotoFrame(2)

drawHLine(3, 'blue')

play(5)

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

# Example 5 - Cycle through frames

Go to next frame in every 500ms.

```
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

