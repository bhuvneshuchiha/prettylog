# 🎨 PrettyLog

[![Go Version](https://img.shields.io/badge/go-%3E%3D1.21-blue.svg)](https://golang.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Go Report Card](https://goreportcard.com/badge/github.com/your-username/prettylog)](https://goreportcard.com/report/github.com/your-username/prettylog)

A beautiful, colorized structured logging package for Go that extends the standard `log/slog` library with enhanced formatting and visual appeal.

## ✨ Features

- 🎨 **Colorized Output** - Automatic color coding for different log levels and components
- 🏷️ **Process & Area Tags** - Organize logs by process and functional area  
- 📊 **Smart Formatting** - Automatically formats attributes in compact or JSON format based on content length
- 🔧 **Flexible Configuration** - Multiple initialization options and environment variable support
- 📝 **Extended Log Levels** - Includes TRACE and FATAL levels beyond standard slog
- 🚀 **Drop-in Replacement** - Compatible with standard `log/slog` interface

## 📦 Installation

```bash
go get github.com/your-username/prettylog
```

## 🚀 Quick Start

### Basic Usage

```go
package main

import (
    "log/slog"
    "os"
    "github.com/your-username/prettylog"
)

func main() {
    // Create a pretty logger using environment variables
    logger := prettylog.CreateLoggerFromEnv(nil)
    
    // Log with process and area context (required)
    logger.Info("Application started", 
        prettylog.ProcessKey, "myapp",
        prettylog.AreaKey, "startup",
        "version", "1.0.0",
        "port", 8080)
}
```

### Manual Initialization

```go
// Create logger with custom parameters
params := prettylog.NewParams(os.Stderr)
logger := prettylog.SetProgramLevelPrettyLogger(params)

// Use all available log levels
prettylog.Trace(logger, "Trace message", 
    prettylog.ProcessKey, "myservice",
    prettylog.AreaKey, "database")

logger.Debug("Debug message",
    prettylog.ProcessKey, "myservice", 
    prettylog.AreaKey, "auth")

logger.Info("Info message",
    prettylog.ProcessKey, "myservice",
    prettylog.AreaKey, "api")

logger.Warn("Warning message",
    prettylog.ProcessKey, "myservice",
    prettylog.AreaKey, "cache")

logger.Error("Error message",
    prettylog.ProcessKey, "myservice", 
    prettylog.AreaKey, "payment")
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DEBUG_TYPE` | Set to `"pretty"` to enable pretty logging | Standard JSON handler |
| `DEBUG_LOG` | File path for log output | `stderr` |
| `NO_PRETTY_LOGGER` | Set to any value to disable pretty logging | Not set |


## 📋 Log Levels

PrettyLog extends the standard slog levels:

| Level | Color | Description |
|-------|-------|-------------|
| `TRACE` | Light Gray | Most verbose logging |
| `DEBUG` | Light Gray | Debug information |
| `INFO` | Cyan | General information |
| `WARN` | Light Yellow | Warning messages |
| `ERROR` | Red | Error messages |
| `FATAL` | Magenta | Fatal errors |

## 📝 Required Fields

**Important**: PrettyLog requires two mandatory fields for all log entries:

- `prettylog.ProcessKey` - Identifies the process/service
- `prettylog.AreaKey` - Identifies the functional area within the process

```go
// ✅ Correct usage
logger.Info("Message", 
    prettylog.ProcessKey, "api-server",
    prettylog.AreaKey, "authentication")

// ❌ This will cause an assertion failure
logger.Info("Message", "some", "value") // Missing ProcessKey and AreaKey
```

## 🎨 Output Examples

### Compact Format (short attributes)
```
api-server:auth INFO User logged in user_id=12345 session=abc123
```

### JSON Format (long attributes)
```
api-server:database DEBUG Query executed {
  "level": "DEBUG",
  "msg": "Query executed",
  "process": "api-server",
  "area": "database", 
  "query": "SELECT * FROM users WHERE id = ? AND status = ? AND created_at > ?",
  "duration_ms": 45,
  "rows_affected": 1
}
```

### Color Coding
- **Process names** get consistent colors (e.g., "sim" = light green, "DummyServer" = light blue)
- **Area names** get automatically assigned unique colors from a rotating palette
- **Log levels** have distinct colors for easy visual scanning

## 🛠️ Advanced Usage

### Using with Different Processes

```go
// Different processes get different colors automatically
logger.Info("API request received",
    prettylog.ProcessKey, "web-server",
    prettylog.AreaKey, "http")

logger.Info("Database query",
    prettylog.ProcessKey, "db-service", 
    prettylog.AreaKey, "postgres")

logger.Info("Cache operation",
    prettylog.ProcessKey, "redis-client",
    prettylog.AreaKey, "cache")
```

### Attribute Smart Formatting

Attributes are automatically formatted based on content length:
- **Short attributes** (≤42 chars): Displayed inline as `key=value`
- **Long attributes** (>42 chars): Displayed as formatted JSON

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built on top of Go's standard `log/slog` package
- Inspired by the need for beautiful, readable logs in development and production

---

**Note**: This package requires Go 1.21+ and uses the `github.com/bhuvneshuchiha/assert` package for assertions.
