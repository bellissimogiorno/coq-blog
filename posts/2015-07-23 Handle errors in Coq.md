Many programming languages handle errors with an exceptions mechanism. There are no exceptions in [Coq](https://coq.inria.fr/), since this is a pure programming language. We mainly get two alternatives:

1. extend Coq with an effect system for exceptions, implemented with monads or alike
2. use explicit sum types

Even if the first option seems more powerful, from my experience an effect system is too heavy for the gains it brings compared the use of sum types with combinators. Sum types are just *simple* and *ubiquitous*. This is in fact the way errors are handled in [Rust](http://blog.burntsushi.net/rust-error-handling/).

They are two basic sum types:

* `option A`: either `Some value` or `None` for an error
* `A + E`: either `inl value` or `inr err` for an error

Since there are no error combinators in the standard library, I wrote the [coq:error-handlers](https://github.com/clarus/coq-error-handlers) library. It provides the basic combinators `bind`, `map` and `default`.

The `bind` sequences two computations with errors:

    Require Import Coq.Lists.List.
    Require Import ErrorHandlers.All.

    Import ListNotations.

    Compute
      Option.bind (List.hd_error [2; 4; 5]) (fun n =>
      Some (n + 1)).

prints `Some 3`.

The `map` is a particular case for when the second expression does not return errors:

    Compute Option.map (List.hd_error [2; 4; 5]) (fun n => n + 1).

prints `Some 3`.

The `default` replaces an error with a default value:

    Compute Option.default (Some 3) 0.
    Compute Option.default None 0.

prints `3` and `0`.

To summarize, here are the types of the combinators:

* `Option.bind : forall {A B}, option A -> (A -> option B) -> option B`
* `Option.map : forall {A B}, option A -> (A -> B) -> option B`
* `Option.default : forall {A}, option A -> A -> A`
* `Sum.bind : forall {E A B}, A + E -> (A -> B + E) -> B + E`
* `Sum.map : forall {E A B}, A + E -> (A -> B) -> B + E`
* `Sum.default : forall {E A}, A + E -> A -> A`

There are no notations to keep things simple and non-intrusive.