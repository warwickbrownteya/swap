# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**swap** is the central coordination hub for the SWAP protocol ecosystem. It provides integration, documentation, and orchestration of all SWAP protocol and SWAP browser components.

### Core Characteristics
- **Type**: Protocol ecosystem hub and coordination point
- **Responsibility**: SWAP protocol integration and coordination
- **Coordinates**: swap-client, swap-server, swap-browser-api (optional), and all swap-client-* UI clients
- **Serves**: Applications using SWAP protocol for semantic-native communication
- **Role**: Central registry and integration point for SWAP ecosystem

## What is SWAP?

SWAP (Semantic Web Application Protocol) is a semantic-native protocol for N3 communication:

- **N3-Native**: All communication in N3 exclusively (no format conversion)
- **Semantic-First**: Protocol operations are semantic, not HTTP-centric
- **Graph Exchange**: Exchange complete N3 graphs atomically
- **Rules First-Class**: Rules are first-class semantic objects
- **Semantic Guarantees**: Completeness, correctness, consistency, determinism
- **Federation**: Transparent federation of semantic servers
- **Direct Access**: No API aggregation layer (unlike HTTP)

## Architecture

**swap is the coordination hub for all SWAP protocol ecosystem components.**

```
swap (Hub)
    ├─ Protocol Layer
    │   ├─ swap-client (SWAP protocol operations)
    │   └─ swap-server (SWAP request handling)
    ├─ Optional API Layer (if needed)
    │   └─ swap-browser-api (Optional: REST wrapping of SWAP)
    ├─ Client Layer (UI Clients)
    │   ├─ swap-client-cli (CLI interface)
    │   ├─ swap-client-tui (Terminal UI)
    │   └─ swap-client-web (Web interface)
    ├─ Coordination
    │   ├─ Integration guide
    │   ├─ Protocol documentation
    │   ├─ Deployment guide
    │   └─ Best practices
    └─ Applications
        └─ End-user applications for SWAP-based semantic web
```

## Component Integration

### Layer 1: Protocol Libraries

```javascript
// swap-client: SWAP protocol operations
const { SwapClient } = require('swap-client');
const client = new SwapClient({
  timeout: 30000,
  semanticCaching: false
});

const result = await client.query({
  serverUrl: "swap://server/endpoint",
  query: "@prefix foaf: <...> .\n?person foaf:knows ?friend ."
  // Returns N3 directly (no format specified)
});
```

```javascript
// swap-server: SWAP request handling
const { SwapServer } = require('swap-server');
const server = new SwapServer({
  port: 8080,
  store: "http://localhost:3030/ds"
});

server.handleQuery({
  query: "...",
  guarantees: {
    completeness: true,
    correctness: true,
    consistency: true,
    determinism: true
  }
});
```

### Layer 2: UI Clients (Direct Access)

```javascript
// swap-client-cli: Direct command-line access
swap-client-cli query "?x a foaf:Person"
swap-client-cli assert "ex:alice foaf:knows ex:bob"

// swap-client-tui: Direct terminal interface
// Fullscreen SWAP client TUI

// swap-client-web: Direct web interface
// Browser-based SWAP client (N3-native communication)
```

## Integration Patterns

### Pattern 1: Direct SWAP Protocol

```javascript
const swap = require('swap');
const { SwapClient } = swap.client;

// Low-level: Direct semantic operations
const client = new SwapClient();

// All results in N3 (no format conversion)
const results = await client.query({
  serverUrl: "swap://server",
  query: "..."
});

// Rules are first-class
const reasoned = await client.reason({
  rules: "{ ?x rdf:type ?c } => { ?x rdf:type ?s } .",
  facts: "..."
});

// Federation is semantic
const federated = await client.federateQuery({
  servers: ["swap://server1", "swap://server2"],
  query: "..."
});
```

### Pattern 2: Via SWAP UI Clients

```javascript
const swap = require('swap');

// Use SWAP UI clients directly
// Each provides semantic web interface

// CLI: Command-line semantic queries
// TUI: Terminal semantic interface
// Web: Browser semantic interface
```

## Core API

### Main Functions

```javascript
swap.client()
  // Get SWAP client library
  // Returns: swap-client module

swap.server()
  // Get SWAP server library
  // Returns: swap-server module

swap.query(serverUrl, queryN3)
  // Execute semantic query
  // Returns: N3 graph (no format parameter)

swap.assert(serverUrl, factsN3)
  // Assert semantic facts
  // Input/output: N3

swap.reason(serverUrl, rulesN3, factsN3)
  // Apply semantic rules
  // Input/output: N3 graphs

swap.federate(servers, queryN3)
  // Semantic federation query
  // Queries multiple servers semantically

swap.getClients()
  // Get all available UI clients
  // Returns: {cli, tui, web}

swap.semanticGuarantees()
  // Get semantic guarantee information
  // Returns: completeness, correctness, consistency, determinism
```

## SWAP vs HTTP Comparison

### Key Differences

```
HTTP Approach:
  ├─ REST endpoints (query, assert, etc.)
  ├─ Multiple response formats (N3, Turtle, JSON-LD, etc.)
  ├─ API aggregation layer (http-browser-api)
  ├─ Format negotiation
  └─ HTTP semantics govern protocol

SWAP Approach:
  ├─ Semantic operations (query, assert, reason, etc.)
  ├─ N3 only (no format negotiation)
  ├─ Direct protocol (no API layer)
  ├─ Semantic guarantees enforced
  └─ Semantic semantics govern protocol
```

## File Structure

### Hub Repository

```
swap/
  ├─ README.md                    # Overview & getting started
  ├─ ARCHITECTURE.md              # Architecture guide
  ├─ INTEGRATION.md               # Integration guide
  ├─ PROTOCOL.md                  # SWAP protocol guide
  ├─ DEPLOYMENT.md                # Deployment guide
  ├─ EXAMPLES.md                  # Code examples
  ├─ package.json                 # Hub metadata
  ├─ src/
  │   ├─ index.js                 # Main entry point
  │   ├─ api.js                   # Hub API
  │   ├─ integration.js           # Integration utilities
  │   └─ constants.js             # Shared constants
  ├─ docs/
  │   ├─ getting-started.md       # Getting started
  │   ├─ protocol-guide.md        # SWAP protocol guide
  │   ├─ semantic-guarantees.md   # Semantic guarantee documentation
  │   ├─ federation-guide.md      # Semantic federation guide
  │   ├─ clients-guide.md         # UI clients guide
  │   ├─ comparison.md            # HTTP vs SWAP comparison
  │   ├─ deployment/              # Deployment guides
  │   │   ├─ docker.md
  │   │   ├─ kubernetes.md
  │   │   └─ on-premise.md
  │   └─ best-practices.md        # Best practices
  └─ examples/
      ├─ query-example.js         # Query example
      ├─ reason-example.js        # Reasoning example
      ├─ federation-example.js    # Federation example
      └─ semantic-guarantee-example.js  # Guarantee example
```

### Component Dependencies

```
swap (Hub)
  └─ Provides unified API over:
      ├─ swap-client (Protocol operations)
      ├─ swap-server (Request handling)
      ├─ swap-client-cli (CLI)
      ├─ swap-client-tui (TUI)
      └─ swap-client-web (Web)
```

## SWAP Protocol Flow

```
User Input (CLI, TUI, Web)
    ↓
swap-client (Direct SWAP operations)
    ├─ Semantic query/assert/reason/etc.
    ├─ N3 input/output
    ├─ Optional federation via swap-client
    ├─ Semantic guarantee enforcement
    └─ Direct to remote SWAP server
    ↓
swap-server (Remote server)
    ├─ Semantic operation handling
    ├─ N3 processing
    ├─ Semantic guarantee verification
    ├─ Optional federation to other servers
    └─ Return N3 results
    ↓
swap-client (Results processing)
    ↓
UI Client (Display N3 results)
    ↓
User sees semantic results
```

## Configuration

### SWAP Ecosystem Configuration

```javascript
const swap = require('swap');

swap.configure({
  client: {
    timeout: 30000,
    maxConnections: 10,
    semanticCaching: false,
    graphValidation: true
  },

  server: {
    port: 8080,
    host: "localhost",
    store: "http://localhost:3030/ds"
  },

  semantic: {
    guarantees: {
      completeness: true,
      correctness: true,
      consistency: true,
      determinism: true
    },
    federation: {
      enabled: true,
      transparentDiscovery: true,
      semanticDeduplication: true
    }
  },

  logging: {
    level: "info",
    includeGraphs: false
  }
});
```

## Semantic Guarantees

### Completeness

All results that should exist are returned

### Correctness

No spurious results (only valid results)

### Consistency

No contradictory results

### Determinism

Same input always produces same output

## Testing

```bash
npm test                      # Run all component tests
npm test:integration        # Integration tests
npm test:semantic           # Semantic guarantee tests
npm test:federation         # Federation tests
npm test:examples           # Example verification
```

## Development

```bash
npm install                 # Install all components
npm run dev                 # Start development mode
npm test                    # Run all tests
npm run build              # Build all components
npm run start:server       # Start SWAP server
npm run start:cli          # Start CLI client
npm run docs               # Generate documentation
```

## Deployment

### Docker

```bash
docker-compose -f docker-compose.yml up
```

### Kubernetes

```bash
kubectl apply -f k8s/
```

## Related Repositories

**Coordinated Components**:
- swap-client (protocol operations)
- swap-server (request handling)
- swap-client-cli (CLI client)
- swap-client-tui (TUI client)
- swap-client-web (Web client)

**Related Ecosystems**:
- http (HTTP protocol ecosystem)
- notation3 (N3 language hub)

**Authority**:
- semantic-native-declaration
- swap-in-http (protocol specification)
- swap-in-notation3 (protocol in N3)

## Authority

- **Semantic Constraints**: semantic-native-declaration
- **Protocol Spec**: swap-in-http, swap-in-notation3
- **Implementation**: Coordination hub for all swap-* repositories

---

**Status**: SWAP protocol ecosystem hub
**Date**: 2026-02-01
