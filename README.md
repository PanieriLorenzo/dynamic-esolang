# dynamic-esolang

> The name is a placeholder

A minimalistic dynamically typed esoteric programming language.

A slightly verbose hello world:
```py
["hello"] = "Hello, World!"
print ["hello"]
```

Fibonacci:
```py
["fibo"] = {
  if ["__args"][0] == 0 or ["__args"][0] == 1
    ["__return"] = 1
    break
  end
  ["__return"] = .(["__args"][0] - 1) + .(["__args"][0] - 2)
}
print ["fibo"](5)
```
