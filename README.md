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

