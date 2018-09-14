# scrale
LÖVE library to scale and center your desired low resolution game the best it can be in desktop window / desktop fullscreen on Mac / PC or in iOS / Android mobile devices based on native resolution [LETTERBOXING]

Your game has to provide a valid conf.lua with `window.width` and `window.height` set, like so:

```lua

    function love.conf(t)
        t.window.width = 512                                             -- This is your game width. Scrale will read it.
        t.window.height = 320                                            -- This is your game height. Scrale will read it.
        -- Interesting: The window will scale up by one or more multipliers of 2 to fit your desktop as best as possible
        t.window.fullscreen = love._os == "Android" or love._os == "iOS" -- Fullscreen on mobile
        t.window.fullscreentype = "desktop"                              -- "desktop" fullscreen is required for scrale
        t.window.hdpi = false                                            -- Disable hdpi by default and let scrale change it for iOS devices
    end

```

Example:

```lua
    
    scrale = require "scrale" -- screen scaler

    function love.load()
        scrale.init({
            fillHorizontally = false, -- fill horizontally on fullscreen
            fillVertically = false, -- fill vertically on fullscreen
            scaleFilter = "nearest",
            scaleAnisotropy = 1,
            blendMode = { "alpha", "premultiplied" },
        }) -- options are optional, defaults are shown here
    end

    function love.draw(dt)
        scrale.drawOnCanvas(true) -- tell it to draw on canvas, true to clear canvas first

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
    local camX, camY = map:convertTileToPixel(34, 28) -- My start tile
    camera:setPosition(camX, camY)

```

To convert clicks to the game resolution, use `scrale.screenToGame(x, y)`, for the opposite use `scrale.gameToScreen(x, y)`. Scrale also has the helper function `scrale.isMobile()` to detect Android or iOS devices.

Have fun!
