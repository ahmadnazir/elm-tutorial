# Structure of Html.App

### Imports

```elm
import Html exposing (Html, div, text)
import Html.App
```

- We will use the `Html` type from the `Html` module, plus a couple of functions `div` and `text`.
- We also import `Html.App` which is the glue that will orchestrate our application. This is the equivalent to StartApp if you have used Elm 0.16. 

### Model

```elm
type alias Model = String

init : (Model, Cmd Msg)
init =
  ( "Hello" , Cmd.none )
```

- First we define our application model as a type alias, in this kind it is just a string.
- Then we define an `init` function, this function provides the initial input for the application. 

__Html.App__ expects a tuple with `(model, command)`. The first element in this tuple is our initial state e.g. "Hello". The second element is an initial command to run, more on this later.

When using the elm architecture we compose all components models into a single state tree. More on this later.

### Messages

```elm
type Msg
  = NoOp
```

Messages are things that happen in our applications and our component responds to. In this case the application doesn't do anything so we only have a messages called `NoOp`.

Another example of messages could be `Expand` or `Collapse` to show and hide a widget. We use union types for messages:

```elm
type Msg
  = Expand
  | Collapse
```

### View

```elm
view : Model -> Html Msg
view model =
  div []
    [ text model ]
```

The function `view` renders an Html element using our application model. Note how the type signature say `Html Msg`, this means that this Html element would produce messages tagged with Msg. We will see this when we introduce some interaction.

### Update

```elm
update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    NoOp ->
      (model, Cmd.none)
```

Next we define an `update` function, this function will be called by Html.App each time a message is received. This update function responds to messages updating the model and returning commands as needed. 

In this example we only respond to `NoOp` and return the unchanged model and `Cmd.none` (meaning no command to perform).

### Subscriptions

```elm
subscriptions : Model -> Sub Msg
subscriptions model =
  Sub.none
```

We use subscriptions to listen for external input on our application. Some examples of subscriptions may be:

- Mouse movements
- Keyboard events
- Browser location changes

In this case we are not interested in any external input so we use `Sub.none`. Note the type signature `Sub Msg`. Subscriptions in a component should all be of the same type.

### Main

```elm
main =
  Html.App.program
    { init = init
    , view = view
    , update = update
    , subscriptions = subscriptions
    }
```

Finally `Html.App.program` wires everything together and returns an html element that we can render in the page. `program` takes our `input`, `view`, `update` and `subscriptions`.






 