Object Updater
==============

These functions 

Apply special properties to scene object.

    w = (string) ->
      string.split(/\s+/)

    SCENE_PROPS = w """
      alpha
      width
      height
      position
      rotation
      scale
      x
      y
      rotation
    """

Run an objects update function if it exists

    update = (object, dt) ->
      object.data.update?(dt)
      applyProperties(object)

Apply all the special properties into the PIXI runtime.

    # TODO: Add `sprite` and `filters` lookups
    applyProperties = (object) ->
      data = object.data
      SCENE_PROPS.forEach (name) ->
        if data[name] != undefined
          object[name] = data[name]

    module.exports =
      update: update
      applyProperties: applyProperties
