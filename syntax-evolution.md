# Syntax evolution

## Int (& Char) ranges 

```res
switch i {
| 0..9 => i
| *..99 as i => i
| _ => i
}
```

```js
if (i >= 0 && i <= 9) {
  //
} else if (i >= 100) {
  //
} else {
  //
}
```

## Iterable & generators

Use primitive type `Iterable.t<'a>`

```
Array.iter: array<'a> => Iterable.t<'a>
List.iter: list<'a> => Iterable.t<'a>
Map.iter: Map.t<'k, 'v> => Iterable.t<('k, 'v)>
```

Or use `gen` keyword.

```res
let count = ref(0)

let genBlock = gen {
  while true {
    count := count.contents + 1
    yield count.contents
  }
}

let genFn = gen () => {
  while true {
    count := count.contents + 1
    yield count.contents
  }
}
```

```js
let count = { contents: 0 };

// Is this pattern GC-safe?
let genBlock = (function*() {
  while (true) {
    count.contents = count.contents + 1;
    yield count.contents;
  }
})();

let genFn = function*() {
  while (true) {
    count.contents = count.contents + 1;
    yield count.contents;
  }
}
```

There would be some built-in generators:

```res
let iter = gen 0..(arr->Array.lastIndex)
let iter = gen 0..* // to Infinity
```


## Foreach loops

This is the best syntax I can imagine now.

```res
let a = foreach (
  iter => v,
  gen 0..* => i,
) {
  break Some(v);
}

```


```js
import * as Primitive_seq from "@rescript/runtime/Primitive_seq";
import * as Primitive_gen from "@rescript/runtime/Primitive_gen";

let a;
for (const [v, i] of Primitive_seq.combine([
  iter,
  Primitive_gen.range(0, Infinity),
])) {
  a = v; break;
}

```

## While loops

```res
let a = while condition {
  break value;
}
```

## Else pattern & early return

```res
let Ok(v) = result else {
  Console.log("Not successful")
  return None
}
Some(v)
```

Even while-pattern loops?

```res
while let Ok(v) = result.contents {
  result := result.contents->next
}
// equavlant to 
while result.contents->Option.isOk {
  let Ok(v) = result.contents else {
    // unrecheable!
  }
  result := result.contents->next
}
```

Hmm... it's less benefit and not chainable.

Only benefit is the exhaustive unwrapping payload.
