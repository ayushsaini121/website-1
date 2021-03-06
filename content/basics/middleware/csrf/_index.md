---
title: "CSRF Middleware"
linkTitle: "CSRF"
weight: 100
---

CRSF middleware is provided by [gorilla/csrf](https://github.com/gorilla/csrf).

```go
import (
    "clevergo.tech/clevergo"
    "github.com/gorilla/csrf"
)
```

```go
m := csrf.Protect(
    []byte("32-byte-long-auth-key"),
    csrf.Secure(false), // developing locally
    // other options
)
app.Use(clevergo.WrapHH(m))
```
