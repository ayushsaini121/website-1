---
title: "请求"
weight: 50
---

> `c.Request` 即 `*http.Request`.

## 请求方法断言

```go
func handler(c *clevergo.Context) error {
	// equals to c.IsMethod(http.MethodGet).
	if c.IsGet() {
	}
	// other shortcuts:
	//c.IsDelete()
	//c.IsPatch()
	//c.IsPost()
	//c.IsPut()
	//c.IsOptions()
	//c.IsAJAX()
	return nil
}
```

## Decode

路由器定义了一个 `Decoder` 接口，用于解码请求的输入。

```go
import "github.com/clevergo/form"
```

```go
type loginForm struct {
	Username string `schema:"username" json:"username"`
	Password string `schema:"password" json:"password"`
}

// Validate implements form.Validatable interface, it will be
// invoked after decoding.
func (f *loginForm) Validate() error {
	// Validates form fields here.
	return nil
}

func login(c *clevergo.Context) error {
	f := new(loginForm)
	if err := c.Decode(f); err != nil {
		return err
	}
	fmt.Printf("%+v\n", f) // &{Username:foo Password:bar}

	return c.Stringf("Hello %s", f.Username)
}

router.Decoder = form.New() // 注册 Decoder
router.Post("/login", login)
```

```shell
$ curl -d "username=foo&password=bar" \
	http://localhost:8080/login
hello foo

$ curl -H "Content-Type: application/json" \
	-d '{"username":"foo", "password":"bar"}' \
	http://localhost:8080/login
hello foo
```

## 快捷方式

| 方法 | |
|---|---|
| Context.Cookie | http.Request.Cookie |
| Context.Cookies | http.Request.Cookies |
| Context.GetHeader | http.Request.Header.Get |
| Context.FormValue | http.Request.FormValue |
| Context.PostFormValue | http.Request.PostFormValue |
| Context.QueryString | http.Request.URL.RawQuery |
| Context.QueryParam | http.Request.URL.Query().Get |
| Context.QueryParams | http.Request.URL.Query() |
| Context.DefaultQuery | QueryParam with default value |
