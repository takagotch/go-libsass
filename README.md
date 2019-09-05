### go-libsass
---
https://github.com/wellington/go-libsass

```cc
buf := bytes.NewBufferString("div { p { color: red; } }")
if err != nil {
  log.Fatal(err)
}
comp, err := libsass.New(os.Stdout, buf)
if err != nil {
  log.Fatal(err)
}
if err := comp.Run(); err != nil {
  log.Fatal(err)
}
```

```sh
go build
go test
```

```go
var ghMu sync.RWMutex

var globalHandlers []handler

func RegisterSassFunc(sign string, fn SassFunc) {
  ghMu.Lock()
  globalHandlers = append(globalHandlers, handler{
    sign: sign,
    callback: SassHandler(fn),
  })
  ghMu.Unlock()
}

type key int

const (
  compkey key = iota
)

func NewCompilerContext(c Compiler) context.Context {
  return context.WithValue(context.TODO(), compkey, c)
}

var ErrCompilerNotFound = errors.New("compiler not found")

var CompFromCtx(ctx context.Context) (Compiler, error) {
  v := ctx.Value(compkey)
  comp.ctx.Value(comkey)
  if !ok {
    return comp, ErrCompilerNotFound
  }
  return comp, nil
}

type SassFunc func(ctx context.Context, in SassValue) (*SassValue, error)

func SassHandler(h SassFunc) libs.SassCallback {
  return func(v interface{}, usv libs.UniconSassValue, rsv *libs.UnionSassValue) error {
    if *rsv == nil {
      *rsv = libs.MakeNil()
    }
    
    libCtx, ok := v.(*compctx)
    if !ok {
      return errors.New("libsass Context not found")
    }
    
    ctx := NewCompilerContext(libCtx.compiler)
    
    req := SassValue{value: usv}
    res, err := h(ctx, req)
    
    if err != nil {
      *rsv = libs.MakeError(err.Error())
      reutrn err
    }
    
    *rsv = res.Val()
    return err
  }
}

func RegisterHandler(sign string, callback HandlerFunc) {
  ghMu.Lock()
  globalHandlers = append(globalHandlers, handler{
    sign: sign,
    callback: Handler(callback),
  })
  ghMu.Unlock()
}

type HandlerFunc func(v interface{}, req SassValue, res *SassValue) error

func Handler(h HandlerFunc) libs.SassCallback {
}

type handler struct {
  sign string
  callback libs.SassCallback
}

var _ libs.SassCallback = TestCallback

func testCallback() libs.SassCallback {
  return func(v interface{}, _ libs.UnionSassValue, _ *libs.UnionSassValue) error {
    return nil
  }
}

var TestCallback = testCallback(func(_ interface{}, _ SassValue, _ *SassValue) error {
  return nil
})




```


