# Prime Development Guide

## Overview

Prime is a CLI tool that depends on the `mcp-server-go` module. Both repositories maintain version parity for compatibility (when one is v0.1.0, the other is also v0.1.0).

## Quick Start

```bash
# Clone repositories if needed
git clone https://github.com/shaneholloman/prime.git
git clone https://github.com/shaneholloman/mcp-server-go.git

# Setup workspace for local development
cd ..  # parent directory containing both repos
go work init
go work use ./prime ./mcp-server-go
```

## Building and Testing

```bash
# Build prime
go build -o prime cmd/prime.go

# Run tests
go test ./...

# Install locally
go install
```

## Making Changes

### Critical Files to Watch

When modifying prime, pay special attention to:

1. **MCP Integration Files**:
   - `cmd/mcp.go`: Core MCP client usage
   - Any files that import mcp-server-go packages

2. **Breaking Changes**:
   - If you modify how prime uses mcp-server-go
   - If you add new MCP client functionality
   Then you MUST:
   1. Test thoroughly with current mcp-server-go version
   2. Verify all MCP client calls still work
   3. Consider if mcp-server-go needs updates

3. **When mcp-server-go Changes**:
   If mcp-server-go updates these files:
   - `mcp/types.go`
   - `client/*.go`
   - `server/*.go`
   Then you MUST update prime's code to match.

### Testing Changes

```bash
# Test prime with local mcp-server-go
go work use ./prime ./mcp-server-go
go test ./...
go run . # Test your changes

# Test with released mcp-server-go
go work sync  # Use released version
go test ./...
```

## Common Commands

```bash
# Run prime
prime

# Check version
prime version

# See available commands
prime --help
```

## Version Management

Prime maintains version parity with mcp-server-go. When releasing:

1. Wait for mcp-server-go to be tagged (e.g., v0.1.0)
2. Tag prime with the same version:

   ```bash
   git tag v0.1.0
   git push origin v0.1.0
   ```

## Troubleshooting

If you encounter issues:

```bash
# Clear Go's module cache
go clean -modcache

# Remove existing prime installation
go clean -i github.com/shaneholloman/prime

# Rebuild dependencies
go mod tidy

# Verify clean installation
go install github.com/shaneholloman/prime@latest
```

## Detailed Documentation

For comprehensive development documentation covering both prime and mcp-server-go, including:

- Detailed troubleshooting steps
- Version management
- Cache handling
- Common issues
- Git commands

See the [mcp-server-go Development Guide](https://github.com/shaneholloman/mcp-server-go/blob/main/DEVELOPMENT.md)
