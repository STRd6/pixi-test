Editor
======

Simple script editor for objects

    Observable = require "observable"

    {hydrate} = require "./object_updater"

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

      self.activeObject.observe (object) ->
        if object
          self.script object.script or ""

      self.script.observe (script) ->
        if object = self.activeObject()
          object.script = script
          hydrate(object)

      return self
