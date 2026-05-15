# AGENTS.md

This file provides guidance to coding agents (e.g. Claude Code, claude.ai/code) when working with code in this repository.

## Repository purpose

Go module `go.virtual-secrets.dev/installer` — Helm charts and supporting tooling for installing the Virtual Secrets project. It also exposes a Go API package describing chart values so that other Virtual Secrets components can consume strongly typed installation parameters.

Two charts are shipped:
- `charts/virtual-secrets-server` — the Virtual Secrets server/operator.
- `charts/secrets-store-csi-driver-provider-virtual-secrets` — the node-level CSI provider DaemonSet.

## Architecture

- `charts/` — one subdirectory per Helm chart. Each has `Chart.yaml`, `values.yaml`, `templates/`, plus generated artifacts `doc.yaml`, `README.md`, and (for charts with a Go-typed schema) `values.openapiv3_schema.yaml` and `crds/`.
- `apis/installer/v1alpha1/` — Go types backing the `values.yaml` of `virtual-secrets-server`. Used both for OpenAPI/values-schema generation and as a typed API surface for downstream programs.
  - `register.go`, `install/`, `fuzzer/` — standard k8s scheme registration and round-trip fuzz helpers.
- `catalog/` — image catalog driven by `kmodules.xyz/image-packer`. `imagelist.yaml` is the source of truth; `copy-images.sh`, `export-images.sh`, `import-images.sh`, `import-into-k3s.sh` are generated. `catalog/README.md` is an auto-generated CVE report — do not hand-edit.
- `hack/scripts/` — helpers for codegen, chart bookkeeping, and release automation (`update-catalog.sh`, `update-chart-dependencies.sh`, `import-crds.sh`, `ct.sh`, `open-pr.sh`, `update-release-tracker.sh`).
- `tests/` — chart-testing (`ct`) configuration / smoke tests.
- `vendor/` — vendored Go deps.
- Single API group/version: `installer:v1alpha1` (`API_GROUPS` in the Makefile).

## Common commands

All Make targets run inside the `ghcr.io/appscode/golang-dev` Docker image — Docker must be running.

- `make ci` — full CI pipeline: `verify check-license lint build unit-tests`. Run before opening a PR.
- `make gen` — regenerate clientset + manifests (`clientset manifests`).
- `make manifests` — regenerate CRDs, chart values schemas, and chart docs (`gen-crds gen-values-schema gen-chart-doc`).
- `make gen-values-schema` — regenerate `values.openapiv3_schema.yaml` from the Go types in `apis/installer/v1alpha1`.
- `make gen-chart-doc` — regenerate per-chart `README.md` (one target per chart subdir under `charts/`).
- `make update-charts` — refresh chart-level metadata (one target per chart subdir).
- `make fmt` — gofmt + goimports across `apis client hack/gencrd`.
- `make lint` — golangci-lint.
- `make unit-tests` / `make test` — Go unit tests.
- `make ct` — run `chart-testing` against the charts.
- `make verify` — `verify-modules` (re-run `gen fmt` and confirm tree is clean).
- `make add-license` / `make check-license` — manage license headers.

Auxiliary helpers (invoked outside Make):

- `./hack/scripts/update-catalog.sh` — regenerate `catalog/` from `imagelist.yaml` via image-packer.
- `./hack/scripts/import-crds.sh` — pull CRDs from dependent repos into the chart `crds/` dirs.
- `./hack/scripts/update-chart-dependencies.sh` — refresh `Chart.lock` / subchart pins.

Run a single Go test (requires a local Go toolchain):

```
go test ./apis/installer/v1alpha1/... -run TestName -v
```

## Conventions

- Module path is `go.virtual-secrets.dev/installer` (vanity URL); imports must use that, not the GitHub URL.
- Edit `apis/installer/v1alpha1/*_types.go` to change a chart's values surface; then run `make gen` so `values.openapiv3_schema.yaml`, the generated clientset, and per-chart `README.md` stay in sync. Do not hand-edit `zz_generated.*.go`, generated chart `README.md` files, `values.openapiv3_schema.yaml`, or anything under `catalog/` except `imagelist.yaml`.
- License: AppsCode (see `LICENSE.md`); use `make add-license` to apply headers to new files.
- Sign off commits (`git commit -s`); contributions follow the project's DCO requirement.
- Vendor directory is checked in — `go mod tidy && go mod vendor` must leave the tree clean (enforced by `verify-modules`).
- `lintconf.yaml` configures the YAML linter used by `make ct`.
