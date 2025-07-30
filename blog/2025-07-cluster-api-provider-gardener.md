---
title: "Announcing cluster-api-provider-gardener: A New Way to Manage Clusters with Cluster API and Gardener"
authors:
  - tobschli
  - LucaBernstein
tags:
  - Cluster API
  - CAPI
  - CAPGa
  - KCP
---

## Announcing cluster-api-provider-gardener: A New Way to Manage Clusters with Cluster API and Gardener

We’re pleased to share that we have published [cluster-api-provider-gardener (CAPGa)](https://github.com/gardener/cluster-api-provider-gardener/), an open-source [Cluster API](https://cluster-api.sigs.k8s.io/) provider that uses Gardener as the underlying platform for cluster lifecycle management.

<!-- truncate -->

### What is cluster-api-provider-gardener (CAPGa)?

CAPGa allows you to manage Kubernetes clusters through Cluster API means, with Gardener acting as the cloud-independent cluster orchestrator.

Specifically, CAPGa implements the Cluster API’s provider interfaces to manage Gardener’s [Shoot clusters](https://gardener.cloud/about/) as Cluster API `Cluster` resources. This enables users to provision, update, and delete clusters managed by Gardener via standard Cluster API tooling and workflows.

![Image showing the interaction of CAPI and Gardener through CAPGa](./static/capi-interaction-gardener-capga.svg)

### What is the difference between Gardener API and Cluster API?

Gardener and Cluster API both aim to automate Kubernetes cluster management, but they approach the problem from opposite directions and with different user audiences in mind.

#### Gardener API: Platform-first and top-down

Gardener is a top-down architected platform, designed to deliver homogeneous Kubernetes clusters across clouds as a Service. It was built with application or service teams in mind who require consistent, production-ready Kubernetes clusters without managing internal details.
- A single manifest defines an entire cluster (`Shoot`) with platform-chosen defaults.
- Users benefit from a user experience layer, sensible defaults, and well-integrated extensions (e.g. DNS, worker pools, cloud-specific settings).
- Cluster lifecycle is abstracted away: versioning, networking, and OS images are handled centrally.
- Clusters can be managed for multiple cloud providers simultaneously.

#### Cluster API: Infrastructure-centric and bottom-up

Cluster API (CAPI), on the other hand, is a bottom-up framework for building cluster management solutions. It’s aimed at infrastructure teams who need fine-grained control over every component of a Kubernetes cluster and know how to assemble them.
- A cluster is defined via multiple resource manifests (Cluster, MachineDeployment, KubeadmConfig, etc.).
- Each resource is reconciled by dedicated controllers.
- There’s no opinionated user interface. Users must understand the internals of Kubernetes cluster bootstrapping.
- Powerful for building your own platform, but requires substantial operational ownership.

You can think of Gardener as a second evolutionary stage of Cluster API: more opinionated, more integrated, and focused on platform-level concerns.

### When to use which?

|                      | Cluster API                          | Gardener API                               |
|----------------------|--------------------------------------|--------------------------------------------|
| Target audience      | Infrastructure / platform engineers  | Application / service teams                |
| Interface style      | Multiple manifests, component-driven | One manifest, opinionated, platform-driven |
| Ownership            | End user manages infrastructure      | Platform team manages infrastructure       |
| Multi-cloud support  | Configure it yourself                | Built-in                                   |
| Lifecycle automation | Assemble from primitives             | Out-of-the-box automation                  |

### Why did we build CAPGa?

Gardener provides a powerful way to manage Kubernetes clusters at scale across many infrastructures. At the same time, Cluster API has become a widely adopted standard for cluster management. CAPGa aims to bridge these two projects, giving users a way to manage Gardener clusters with Cluster API-compatible controllers and tools.

This unlocks several new use cases:
- Users familiar with CAPI tooling can now manage Gardener-based clusters.
- Integrators can plug Gardener into existing Cluster API-based workflows.
- Platforms can gradually move towards a unified control plane managed by Gardener, while still using familiar CAPI resources.

### Key Features

- **Cluster API compatibility**: Use familiar Cluster API resources to create and manage Kubernetes clusters.
- **Still enjoy Gardener benefits**: Leverage Gardener’s existing support for various infrastructures, automatic version updates, hibernation and much more.

### Demo

![demo](./static/demo.gif)

### KCP support

CAPGa comes with built-in [kcp](https://www.kcp.io/) support, allowing users to interact with a shared, multi-tenant control-plane that is purpose-built for Kubernetes-like APIs beyond traditional container workloads.

The long-term goal is to support CAPGa within a centrally managed _Platform Mesh_ built on top of kcp. This approach eliminates the need for users to provision and operate a dedicated Kubernetes cluster solely to host their Cluster API components. At the same time, users can interact directly with the Gardener API through the same unified control plane, enabling flexible and consistent cluster lifecycle management across both interfaces. This scenario is also illustrated in the image below.

![Transition from managing Cluster API from a custom cluster to Platform Mesh](./static/capi-transition-custom-platform-mesh.svg)

### Contributing

We encourage anyone interested to try out CAPGa and share your experiences. If you encounter issues or have ideas for improvements, please open an issue or a pull request in the [GitHub repository](https://github.com/gardener/cluster-api-provider-gardener). Contributions are very welcome.
