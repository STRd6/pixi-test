Pixi Test
=========

Testing out Pixi.js

    TAU = 2 * Math.PI
    _ = require "./lib/underscore"
    {extend, pick} = _
    
    {applyStylesheet} = require "util"
    applyStylesheet require("./style")

    PIXI = require "./lib/pixi"

    stage = new PIXI.Stage(0x66FF99)

    renderer = PIXI.autoDetectRenderer(400, 300)

    document.body.appendChild(renderer.view)

Load textures from a data file and map them into Pixi.js texture objects

    textures = require "./textures"
    Object.keys(textures).forEach (name) ->
      value = textures[name]
      textures[name] = PIXI.Texture.fromImage("http://a0.pixiecdn.com/starwipe/#{value}", true)

Reload our app data or use our default data.

    data = ENV?.APP_STATE or require("./default_data")

Reconstitute our objects using our app data.

    objects = data.map (datum) ->
      object = new PIXI.Sprite(textures[datum.sprite])

      console.log object.sprite

      extend object, datum

      stage.addChild(object)

      return object

Our main loop, update and draw.

    animate = ->
      requestAnimationFrame(animate)

      objects.forEach (object) ->
        object.rotation += TAU / 60

      renderer.render(stage)

    requestAnimationFrame(animate)

This is where we export and expose our app state.

    global.appData = ->
      stage.children.map (child) ->
        pick child, "sprite", "position", "anchor", "rotation"

    console.log JSON.stringify(appData(), null, 2)
