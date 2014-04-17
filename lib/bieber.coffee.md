Bieber
======

Tween it!

Data is a table of prop, value list pairs

>  x: [
>    [time, value]
>    [...]
>  ]

    module.exports = (data={}) ->
      apply: (object, t) ->
        enumerate data, (property, tValues) ->
          

Helpers
-------

    enumerate = (object, fn) ->
      Object.keys(object).forEach (name) ->
        fn(name, object[name])
