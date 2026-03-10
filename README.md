# vibe-c2-golang-protocol

Shared Go protocol contracts for Vibe C2.

## Install

```bash
go get github.com/vibe-c2/vibe-c2-golang-protocol@latest
```

## Quick Usage

```go
package main

import (
	"errors"
	"fmt"
	"time"

	"github.com/vibe-c2/vibe-c2-golang-protocol/protocol"
)

func main() {
	msg := protocol.InboundAgentMessage{
		MessageID: "msg-001",
		Type:      protocol.TypeInboundAgentMessage,
		Version:   protocol.VersionV1,
		Timestamp: time.Now().UTC().Format(time.RFC3339),
		Source: protocol.SourceInfo{
			Module:         "agent",
			ModuleInstance: "agent-1",
			Transport:      "ws",
			Tenant:         "default",
		},
		ID:            "agent-123",
		EncryptedData: "base64-or-ciphertext",
		Meta: protocol.MessageMeta{
			"trace_id": "trace-1",
		},
	}

	if err := protocol.ValidateInbound(msg); err != nil {
		var vErr *protocol.ValidationError
		if errors.As(err, &vErr) {
			fmt.Printf("validation failed: code=%s message=%s\n", vErr.Code, vErr.Message)
		}
	}
}
```
