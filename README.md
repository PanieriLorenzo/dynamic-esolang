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