# scrale
LÖVE library to scale and center your desired low resolution game the best it can be in desktop window / desktop fullscreen on Mac / PC or in iOS / Android mobile devices based on native resolution [LETTERBOXING]

Example:

```lua
scrale = require "lib.scrale" -- screen scaler

function love.load()
    scrale.init(800, 600, { -- use the same like in conf.lua window settings, but configure conf.lua to use fullscreentype "desktop"
  		fillHorizontally = false, -- fill horizontally on fullscreen
  		fillVertically = false, -- fill vertically on fullscreen
  		scaleFilter = "nearest",
  		scaleAnisotropy = 1,
	  }) -- options are optional, defaults are shown here
    -- Interesting: The window will scale up by one or more multipliers of 2 to fit your desktop as best as possible
    -- Also interesting: This lib will work on mobile, too. (Incl. Android)
end

function love.draw(dt)
    scrale.drawOnCanvas() -- tell it to draw on canvas

    -- draw anything here

    scrale.draw() -- draw the parent canvas, this will letterbox your game as you wish
end

function love.keypressed(k)
    if k == "+" then -- decide yourself what key you like ...
        scrale.toggleFullscreen() --  ... and change fullscreen if you want (there is also setFullscreen, check the code)
    end
end

```

How to let it work with STI and Gamera, quick example:

```lua
    local mW, mH = map:convertTileToPixel(map.width, map.height)
    local camera = gamera.new(0, 0, mW, mH)
    local cW, cH = scrale.getCanvasSize()
    camera:setWindow(0, 0, cW, cH)
    camera:setScale(2) -- I wish me a greater view
    local camX, camY = map:convertTileToPixel(34, 28) -- My start tile
    camera:setPosition(camX, camY)
```

To convert clicks to the game resolution, use `scrale.screenToGame(x, y)`, for the opposite use `scrale.gameToScreen(x, y)`

Have fun!
