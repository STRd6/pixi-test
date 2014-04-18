Editor
======

Simple script editor for objects

    Observable = require "observable"

    module.exports = ->
      self =
        script: Observable ""
        activeObject: Observable null
        error: Observable ""

      self.view = require("./templates/editor")(self).children[0]
      document.body.appendChild self.view

      # TODO: Figure out how to add class better in hamljr
      self.activeObject.observe (newValue) ->
        if newValue
          self.view.classList.add("active")
        else
          self.view.classList.remove("active")

      self.script.observe (script) ->
        try
          compiled = Function CoffeeScript.compile script, bare: true
        catch error
          self.error error
          console.error error

        if (data = self.activeObject()?.data) and compiled
          data.script = script
          compiled.call data

      return self
