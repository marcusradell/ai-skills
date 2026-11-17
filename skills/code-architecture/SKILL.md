---
name: code-architecture
description: This skill describes how to shape the code to isolate changes to the system.
---

# Code Architecture

A good code architecture minimizes the cost of change. Cost of change is driven by how much code needs to be updated for an atomic change to be valid.

## Infrastructure drivers

Infrastructure drivers wrap things like databases, loggers, routers, servers, file systems, etc. Anything related to input/output.

Each infrastructure driver should be able to work locally and in production. A local implementation can be offered when needed. The local variant should work for automated tests as well.

The driver should not contain business logic, but leave that to the feature slices.

## Feature slices

A feature slice forms a stand-alone solution to a business problem. A feature slice should minimize coupling against other feature slices. The internal code is further separated into horizontal layers.

A feature slice can be part of the application core, the domain, or be a supporting feature.

The core features take care of things that other domain features need to repeatedly perform, but aren't strictly infrastructure drivers. The core features form the bottom layer of features.

The domain features can use the core features. They make up the middle layer of features.

The supporting features cannot have business logic of their own and can only compose other features together.

### Cross-slice communication

Prefer wiring slices together in the compositional root for simplicity.

Prefer a single write and multiple reads across slices. When writes must be coordinated across slice boundaries, use the transactional outbox pattern.

## Horizontal layers

Layers from top to bottom:

1. Transport layers: Routing and Seed.
2. Compositional root.
3. Service layer.
4. Validation, Logic, and Repository layers.

The Service layer orchestrates and calls the Validation, Logic, and Repository layers directly. Those three layers never call each other.

Each layer should offer a factory to describe what it needs injected.

### Transport layers

Transport layers are separate binaries that use the compositional roots.

The Routing layer creates HTTP endpoints for the service methods.

The Seed layer uses the composed service to seed data that feels like real production data.

### Compositional root

The compositional root is a feature slice's top level layer. It composes the service layer with infrastructure drivers and with other feature slices' service layers.

A compositional root should visualize coupling to expose code smells. It should depend on singular functions over objects with methods attached.

### Service layer

The Service layer exposes the feature slice's public methods and orchestrates the layers below it.

### Validation layer

The Validation layer validates raw input to the service methods.

### Logic layer

The Logic layer consists of pure functions containing the business logic. No input/output.

### Repository layer

The Repository layer wraps the database client and creates service-specific public methods that the Service layer consumes.

## Errors

Drivers have their own error types. Driver errors are normalized to feature errors. Feature errors are converted into application errors by the transport layers.

## Layout

Follow the recommended Rust project layout in general.

- Feature slices live in `features/<slice>`.
- Infrastructure drivers live in `drivers/<driver>`.
- Transport layers are separate binaries.
- Each method implementation has its own file, so methods don't depend on each other.
- Tests are co-located with the code they test.

## Testing

Map the layers to the test layers in the tdd skill:

- Logic layer: unit tests against pure functions. No mocks.
- Service, Validation, Repository layers, and drivers: integration tests against local infrastructure.
- Business: observability against production infrastructure.
