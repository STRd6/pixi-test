Pixi Test
=========

Testing out Pixi.js

    TAU = 2 * Math.PI
    _ = require "./lib/underscore"
    {extend, pick, debounce} = _

    {applyStylesheet} = require "util"
    applyStylesheet require("./style")
    {width, height} = require "./pixie"

    {update, applyProperties} = require "./object_updater"
    editor = require("./editor")()

    PIXI = require "./lib/pixi"

    stage = new PIXI.Stage(0x66FF99)

    renderer = PIXI.autoDetectRenderer(width, height)

    clickHandler = (mouseData) ->

      if mouseData.originalEvent.ctrlKey
        editor.activeObject mouseData.target

    document.body.appendChild(renderer.view)

Load textures from a data file and map them into Pixi.js texture objects

    textures = require "./textures"
    Object.keys(textures).forEach (name) ->
      value = textures[name]
      textures[name] = PIXI.Texture.fromImage("http://a0.pixiecdn.com/starwipe/#{value}", true)

Reload our app data or use our default data.

    if data = ENV?.APP_STATE
      data = JSON.parse(data)
    else
      data = require("./default_data")

Reconstitute our objects using our app data.

    objects = data.map (datum) ->
      object = new PIXI.Sprite(textures[datum.sprite])

      object.data = datum
      object.interactive = true
      object.click = clickHandler

      applyProperties(object)
      stage.addChild(object)

      return object

Our main loop, update and draw.

    dt = 1/60
    animate = ->
      requestAnimationFrame(animate)

      objects.forEach (object) ->
        update(object, dt)

      renderer.render(stage)

    requestAnimationFrame(animate)

This is where we export and expose our app state.

    global.appData = ->
      JSON.stringify stage.children.map (child) ->
        child.data

    console.log JSON.stringify(appData(), null, 2)
