Pixi Test
=========

Testing out Pixi.js

    PIXI = require "./lib/pixi"

    stage = new PIXI.Stage(0x66FF99)

    renderer = PIXI.autoDetectRenderer(400, 300)

    document.body.appendChild(renderer.view)

    texture = PIXI.Texture.fromImage("http://a0.pixiecdn.com/starwipe/64edba3e616b2c0d6f87517bc929f1b83c7a6647", true)
    bunny = new PIXI.Sprite(texture)

    bunny.anchor.x = 0.5
    bunny.anchor.y = 0.5

    bunny.position.x = 200
    bunny.position.y = 150

    stage.addChild(bunny)

    animate = ->
      requestAnimationFrame(animate)

      bunny.rotation += 0.1

      renderer.render(stage)

    requestAnimationFrame(animate)
