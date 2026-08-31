# container-build

Status: design-only. This repository does not contain an executable build or
release pipeline.

This repository defines the control plane for Local Inference Lab container
images. The design resolves source and dependency inputs into immutable build
locks, builds each OCI platform on a native executor, verifies each runnable
image by digest, and publishes a multi-platform index only after every required
platform passes.

## Design

See [Container Image Build Process](docs/image-build-process.md) for the
architecture, artifact contracts, release flow, OCI attestation handling, and
extension boundary for additional image families.

## Scope

The initial image family is the Local Inference Lab
[vLLM fork](https://github.com/local-inference-lab/vllm), published for
`linux/amd64` and `linux/arm64`. SGLang follows the same control-plane contract
while retaining its own source-owned build recipe and verification policy.

This repository owns build policy, resolved locks, image-family adapters,
verification orchestration, and release-index assembly. Source repositories
own their Dockerfiles and dependency locks. Deployment repositories own model,
quantization, topology, performance, and rollback qualification.

## Security boundary

The repository is public. Pull requests may validate documentation and static
configuration on GitHub-hosted runners, but they must not execute source
Dockerfiles on persistent Local Inference Lab hosts. Native builds and GPU
verification run only through a trusted executor using reviewed build locks.
