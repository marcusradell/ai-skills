---
name: code-architecture
description: This skill describes how to shape the code to isolate changes to the system.
---

# Code Architecture

A good code architecture minimizes the cost of change. Cost of change is driven by how much code needs to be updated for an atomic change to be valid.

## Infrastructure drivers

Infrastructure drivers will wrap things like databases, loggers, routers, servers, file systems, etc. Anything related to input/output.

Each infrastructure driver should be able to work locally and in production. A local implementation can be offered when needed. The local variant should work for automated tests as well.

The driver should not contain business logic, but leave that to the feature slices.

## Feature slices

A feature slice forms a stand-alone solution to a business problem. A feature slice should minimize coupling against other feature slices. The interal code is further separated into horizontal layers.

A feature slice can be part of the application core, the domain, or be a supporting feature.

The core features takes care of things that other domain features need to repeatedly perform, but isn't strictly an infrastructure driver. The core features form a bottom layer of features.

The domain features can use the core feature. They make up the middle layer of features.

The supporting features can not have business logic of their own and can only compose other features together.

## Horizontal layers

Horizontal layers can only call the layer directly underneath it.
Each layer should offer a factory to describe what it needs injected.

Compositional root is a feature slice's top level layer that connects with other feature slice's service layer, and with infrastructure. A compositional root should visualize coupling to expose code smells. It should depend on singular functions over objects with methods attached.

Service layer
Validation layer...
Logic layer...
Repository layer...
Seed layer...
Routing layer...
