Pixi Test
=========

Testing out Pixi.js

    TAU = 2 * Math.PI
    _ = require "./lib/underscore"
    {extend, pick} = _

    PIXI = require "./lib/pixi"

    stage = new PIXI.Stage(0x66FF99)

    renderer = PIXI.autoDetectRenderer(400, 300)

    document.body.appendChild(renderer.view)

    texture = PIXI.Texture.fromImage("http://a0.pixiecdn.com/starwipe/64edba3e616b2c0d6f87517bc929f1b83c7a6647", true)

    data = ENV?.APP_STATE
    data ||= [
      position:
        x: 200
        y: 150
      anchor:
        x: 0.5
        y: 0.5
      rotation: 0
    ]

    objects = data.map (datum) ->
      object = new PIXI.Sprite(texture)

      extend object, datum

      stage.addChild(object)

      return object

    animate = ->
      requestAnimationFrame(animate)

      objects.forEach (object) ->
        object.rotation += TAU / 60

      renderer.render(stage)

    requestAnimationFrame(animate)

    global.appData = ->
      stage.children.map (child) ->
        pick child, "position", "anchor", "rotation"
