# dynamic-esolang

> The name is a placeholder

A minimalistic dynamically typed esoteric programming language.

A slightly verbose hello world:
```rb
["hello"] = "Hello, World!"
print ["hello"]
```

Fibonacci:
```rb
["fibo"] = {
  if ["__args"][0] == 0 or ["__args"][0] == 1
    ["__return"] = 1
    return
  end
  ["__return"] = .(["__args"][0] - 1) + .(["__args"][0] - 2)
}
print ["fibo"](5)
```

Canonical FizzBuzz:
```rb
["i"] = 0
loop
  ["i"] += 1
  if ["i"] % 15 == 0
    print "FizzBuzz"
    continue
  end
  if ["i"] % 3 == 0
    print "Fizz"
    continue
  end
  if ["i"] % 5 == 0
    print "Buzz"
  end
  if ["i"] >= 100
    break
  end
end
```

Triangle:
> Determine if a triangle is equilateral, isosceles, or scalene.
```rb
["triangle_type"] = {
  ["a"] = ["__args"][0]
  ["b"] = ["__args"][1]
  ["c"] = ["__args"][2]

  # check if it is a valid triangle
  if ["a"] == 0 or ["b"] == 0 or ["c"] == 0
    ["__return"] = null
    return
  end
  if not (["a"] + ["b"] >= ["c"] and ["a"] + ["c"] >= ["b"] and ["b"] + ["c"] >= ["d"])
    ["__return"] = null
    return
  end
  
  # given that the trianlge is valid, do the rest
  if ["a"] == ["b"] and ["a"] == ["c"] and ["b"] == ["c"]
    ["__return"] == "equilateral"
    return
  end
  if ["a"] == ["b"] and ["a"] != ["c"] or ["a"] == ["c"] and ["a"] != ["b"] or ["b"] == ["c"] and ["a"] != ["b"]
    ["__return"] == "isosceles"
    return
  end
  if ["a"] != ["b"] and ["b"] != ["c"]
    ["__return"] == "scalene"
    return
  end
}
```

## Types

There are only a handful of atomic types:
- `int`
- `float`
- `bool`
- `str`
- `null`

There is only one data structure:
- `obj`

An `obj` is a multi-purpose container that does many different things:
- it is a dictionary / associative array
- it is a list
- it is a ~~function~~ procedure

Here it is used as a dictionary:
```rb
{
  ["red"] = 0xff0000
  ["green"] = 0x00ff00
  ["blue"] = 0x0000ff
}
```

Here it is used as a list:
```rb
["primes"] = {
  [0] = 2
  [1] = 3
  [2] = 5
  [3] = 7
  [4] = 11
  [5] = 13
}
```

Here it is used as a procedure:
```rb
["square"] = {
  ["__return"] = ["__args"][0] * ["__args"][0]
}
```

It all depends on how you use it! For example, to use it as a procedure, we need to use the evaluation operator `()`:
```rb
print ["square"](5)
# > 25
```

If we instead assume that it is a dictionary, we might access it like so, and it would not really work:
```rb
print ["square"]["__return"]
# > null
```
That's because the special field `__args` is not set unless we use the evaluation operator, at which point it is set to the contents of the evaluation's attributes (in this case `5`).

## Casting

There is no explicit type casting in DL, instead we rely on the interpreter guessing what the correct casting is, which will often be wrong unless we are very careful.

These are (most of) the casting rules, see if you can remember them all! Hint, the `@` operator is used here as a placeholder for all binary arithmetic operators `+`, `-`, `*`, `/`. If A @ B and B @ A are the same, the second is omitted for brevity. `Any` represents any type. If it isn't mentioned here, it probably gives `null`.

| expression | result | example | reasoning |
| --- | --- | --- | --- |
| `bool @ null` | `int` | `true + null = 1` | null is 0 in most cases |
| `int @ null` | `int` | `100 + null = 100` | // |
| `float @ null` | `float` | `1.1 + null = 1.1` | // |
| `bool @ bool` | `int` | `true + true = 2` | bools are just 1-bit ints |
| `bool @ int` | `int` | `true + 2 = 3` | // |
| `bool @ float` | `float` | `true + 1.1 = 2.1` | //
| `bool + str` | `str` | `true + "hello" = "truehello"` | string concatenation |
| `int + str` | `str` | `42 + "hello" = "42hello"` | // |
| `float + str` | `str` | `1.1 + "hello" = "1.1hello"` | // |
| `bool[Any]` | `null` | `true[1] = null` | a simple value has no members |
| `bool()` | `null` | `true(42) = null` | there are no contents to evaluate |
| `-bool` | `int` | `-true = -1` | seemed right?? |
| `~bool` | `int` | `~true = a_very_big_number` | why not? |
| `obj @ Any` | `null` | ... | nonsensical |
| `obj + obj` | `obj` | `{["a"] = 0} + {["b"] = 1} = {["a"] = 0; ["b"] = 1}` | additive merge |
| `float + int` | `float` | `1 + 1.1 = 2.1` | casting towards most precise |

In general, the types will tend towards this when combined:

```mermaid
graph LR

null --> bool --> int --> float --> str
null --> int -> str
null --> float
null --> str
bool --> str
bool --> float
str --> str
obj --> null
obj --> obj
```

## Error Handling (or lack thereof)

There is no error handling in DL. The language does not have undefined behavior (unless the devs fucked it up), nor does it have any run-time memory errors beside out of memory errors. In any other case the interpreter will silently guess the right answer even where there isn't one. Usually this results in some strange casting shenanigans or everything turning into `null`. There are almost no semantic checks at compile time.

For example, if I reference a variable that does not exist, its value will simply be `null`. Or if I assign to a child of a variable that does not exist, the parent will simply pop into existence without warning. Example:

```rb
print ["a"]
# > null
["a"]["b"]["c"] = 42
print ["a"]
# > {"a": {"b": {"c": 42}}}
```

In the rare cases where there truly is no other choice, the program will crash with no useful error message.

So to handle errors, you need to figure a system out for yourself with what you are given, for example checking the types of variables in a paranoid manner. Careful not to make typos! That's right, types are just strings with the name of the type.

```rb
["a"] = 42
print ["std"]["type"](["a"]) == "int"
# > true
print ["std"]["type"](["a"]) == "Int"
# > false
```
