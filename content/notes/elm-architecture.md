---
title: "Elm Architecture"
date: 2021-09-18T00:16:44-04:00
math: true
type: "note"
draft: false
---

`main` value is what gets shown on screen. Think of it as the high-level description of our program.

```elm
main = 
    Browser.sandbox { init = init, update = update, view = view }
```

`model` captures all the detail as data.
Example: Use an `Int` value to track the current count. We can see that in our initial value: 0.

```elm
type alias Model = Int

init : Model
init =
    0
```

`view` displays the `Model` on the screen.
Example:
    - Takes in `Model` as an arg and outputs HTML.
    - It says we want to show a dec btn, count, inc btn.
    - `onClick` handler for each button says when clicked, generate msg dec, inc, respectively

```elm
view : Model -> Html Msg
view model =
    div []
        [ button [ onClick Decrement ] [ text "-" ]
        , div [] [ text (String.fromInt model) ]
        , button [ onClick Increment ] [ text "+" ]
        ]
```

`update` describes how our `Model` will change over time. 

Example:
    - first, define 2 messages we might get
    - if we get `Increment` msg, +1 otherwise -1

```elm
type Msg = Increment | Decrement

update : Msg -> Model -> Model
update msg model = 
    case msg of
        Increment ->
            model + 1

        Decrement ->
            model - 1
```

In summary, every msg runs through update to get a new model, then we call view to show the new model on screen, then repeat. User input generates a msg, updates model, view it on screen, ...

Elm starts by rendering the initial value on screen. From there you enter into this loop:

1. Wait for user input.
1. Send a message to update
1. Produce a new Model
1. Call view to get new HTML
1. Show the new HTML on screen
1. Repeat!
