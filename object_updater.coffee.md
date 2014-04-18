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

Run an objects update function if it exists.

    update = (hostObject, dt) ->
      hostObject.data.update?(dt)
      applyProperties(hostObject)

Apply all the special properties into the PIXI runtime.

    # TODO: Add `sprite` and `filters` lookups
    applyProperties = (hostObject) ->
      data = hostObject.data
      SCENE_PROPS.forEach (name) ->
        if data[name] != undefined
          hostObject[name] = data[name]

    hydrate = (object) ->
      try
        compiled = Function CoffeeScript.compile object.script, bare: true
      catch error
        console.error error

      if compiled
        compiled.call object

    module.exports =
      hydrate: hydrate
      update: update
      applyProperties: applyProperties
