# gleatter_lustre

Interface for lustre to use gleatter's typed routes

[![Package Version](https://img.shields.io/hexpm/v/gleatter_lustre)](https://hex.pm/packages/gleatter_lustre)
[![Hex Docs](https://img.shields.io/badge/hex-docs-ffaff3)](https://hexdocs.pm/gleatter_lustre/)

```sh
gleam add gleatter_lustre
```
```gleam
import gleatter/lustre as glustre
import gleam/http
import lustre
import lustre/effect

pub fn main() {
  let app = lustre.application(init, update, view)
  let assert Ok(_) = lustre.start(app, "#app", Nil)

  Nil
}

fn update(model: Model, msg: Msg) -> #(Model, Effect(Msg)) {
  case msg {
    TodoAdded(title) -> #(model, glustre.create_factory()
      |> glustre.with_scheme(http.Http)
      |> glustre.with_host("localhost")
      |> glustre.with_port(2345)
      |> glustre.for_route(todo_service() |> create_route)
      |> glustre.with_body(CreateTodo(title))
      |> glustre.send(ServerCreatedTodo, fn(_) { effect.none() })
    )
    ServerCreatedTodo(Ok(new_todo)) -> #([new_todo, ..model], effect.none())
    _ -> #(model, effect.none())
  }
}
```

Further documentation can be found at <https://hexdocs.pm/gleatter_lustre>.

## Development

```sh
gleam run   # Run the project
gleam test  # Run the tests
```
