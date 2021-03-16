# Sumopixel
Documentation updated 1.3.2021

### Code Editor Docs

The idea is that you can write JavaScript as you like and access the same features of the Pixel that are being used by the editor itself. So we have exposed some of the variables and functions into window scope, which means you can access them in the code window.

# Pixel object
Here are the variables available in the code window

## Vars

- `gridSize` - If grid size is 32 x 32 then this value is `1024` etc.
- `gridColumns` - Number of columns (or rows) in the grid.
- `width` - same as gridColumns.
- `height` - same as gridColumns.
- `brushColor` - Color code of the currently selected color, eg `rgb(255, 255, 255)`
- `frames` - total number of frames (counted starting from zero)
- `currentFrame` - number of the current frame (starts from zero)
- `usedColors` - array of color codes that has been selected from color picker (visible on toolbar).
- `gridColors` - array of color codes that are present in the grid.

## Methods

- [setPixel](https://github.com/sumo-apps/pixel/blob/main/README.md#setpixel)
- [setPixelByIndex](https://github.com/sumo-apps/pixel/blob/main/README.md#setpixelbyindex)
- [Pixel.onPress](https://github.com/sumo-apps/pixel/blob/main/README.md#pixelonpress)
- [invertColor](https://github.com/sumo-apps/pixel/blob/main/README.md#invertcolor)
- [replaceColor](https://github.com/sumo-apps/pixel/blob/main/README.md#replacecolor)
- [drawHLine](https://github.com/sumo-apps/pixel/blob/main/README.md#drawhline)
- [drawVLine](https://github.com/sumo-apps/pixel/blob/main/README.md#drawvline)
- [play](https://github.com/sumo-apps/pixel/blob/main/README.md#play)
- [gotoFrame](https://github.com/sumo-apps/pixel/blob/main/README.md#gotoframe)
- [addFrame](https://github.com/sumo-apps/pixel/blob/main/README.md#addframe)
- [copyFrame](https://github.com/sumo-apps/pixel/blob/main/README.md#copyframe)
- [wait](https://github.com/sumo-apps/pixel/blob/main/README.md#wait)
- [shift](https://github.com/sumo-apps/pixel/blob/main/README.md#shiftup-shiftdown-shiftleft-shiftright)
- [clear](https://github.com/sumo-apps/pixel/blob/main/README.md#clear)
- [clearFrame](https://github.com/sumo-apps/pixel/blob/main/README.md#clearframe)
- [clearAll](https://github.com/sumo-apps/pixel/blob/main/README.md#clearall)
- [synth](https://github.com/sumo-apps/pixel/blob/main/README.md#synth)
- [random](https://github.com/sumo-apps/pixel/blob/main/README.md#random)
- [animate](https://github.com/sumo-apps/pixel/blob/main/README.md#animate)
- [notify](https://github.com/sumo-apps/pixel/blob/main/README.md#notify)

### setPixel
`setPixel(x, y, color-or-image)`

Sets pixel color or image of given x, y coordinate.\
For example `setPixel(10, 10, 'rgb(255, 255, 255)')` sets pixel at 10, 10 (x,y) into white.\
or\
`setPixel(10, 10, 'https://sumo.app/images/icons/sumo-icon-64.png')` sets pixel at 10, 10 (x,y) into Sumo logo.

### setPixelByIndex
`setPixelByIndex(index, color-or-image)`

Sets pixel color or image at given index (0 - total amount of pixels).\
For example `setPixelByIndex(64, 'rgb(255, 255, 255)')`
or\
`setPixelByIndex(64, 'https://sumo.app/images/icons/sumo-icon-64.png')`

### Pixel.onPress
`Pixel.onPress = function`

Gets triggered when pixel on a grid is clicked on and returns an object with the pixel index, column, row and color.\
For example:\
`Pixel.onPress = (pixel) => console.log(pixel)`\
outputs:
```
{
  index: number,
  column: number,
  row: number,
  color: string
}
```
\
Then you could turn that pixel white by doing this:\
`Pixel.onPress = (pixel) => setPixelByIndex(pixel.index, 'white')`\
\
or this..\
\
`Pixel.onPress = (pixel) => setPixel(pixel.column, pixel.row, 'white')`

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

### deleteFrame

`deleteFrame(frame)` - Deletes the given frame.\
\
PS. Notice that after the frame has been deleted `frames` array has also changed.

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

### synth
Synthetizer (ToneJS) Experimental!\
For example: `synth.triggerAttackRelease('C4', '16n')`
More documentation: https://tonejs.github.io/docs/14.7.77/Synth

### random
`random(max)`\
Return random value between zero and given max value.

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
let index = (gridSize / 2) + (width / 2)

setPixelByIndex(index, 'white')

window.focus()

window.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowUp') {
    clearFrame()
    index = index - width
    setPixelByIndex(index, 'white')
  }

  if (e.key === 'ArrowDown') {
    clearFrame()
    index = index + width
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

