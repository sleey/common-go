# common-go
Wrapper for common go library. Help you to reduce boilerplate code for initialization. Init once and use it everytime in your code.

Included library:
1. Monitoring (Datadog)
2. Tracker (Jaeger + Open Tracing)
3. Logging (Logrus)
4. Config Reader (Viper)

## How to use
1. Import your need to your Golang code
```go
package main

import (
    log "github.com/sleey/common-go/log"
    datadog "github.com/sleey/common-go/datadog"
    config "github.com/sleey/common-go/config"
    tracer "github.com/sleey/common-go/tracer"
)
```

2. Init the library
```go
package main

import (
    config "github.com/sleey/common-go/config"
    log "github.com/sleey/common-go/log"
    datadog "github.com/sleey/common-go/datadog"
    tracer "github.com/sleey/common-go/tracer"
)

func init() {
    // init config example
    err := config.NewConfigFromFile("<config_name>", "<config_extension>", "<file_path>", config.NewConfigOptions{
		DefaultName: "<config_default_name>",
    })
    
    // init log example
    log.SetLogConfig(log.Config{
        LogLevel:  "debug",
        ShortPath: false,
    })

    // init datadog example
	datadog.InitDatadog(datadog.Config{
		Endpoint:    "<datadog_host>",
		ServiceName: "<service_name>",
		DefaultTags: []string{"env:production"},
    })
    
    tracer.Initialize(tracer.Config{
        ServiceName: "<service_name>",
    })
}
```

3. Use the library
```go
func main() {
    log.Info("====== COMMON-GO =======")
    test := config.GetString("test.config")
    datadog.Histogram(test, time.Since(now).Seconds(), []string{"test"}, 1)
    tracer.StartSpanFromContext(context.Background())
    log.Info("END")
}
```

## License
MIT License