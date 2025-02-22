# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Added

- Basic benchmarking

### Changed

-

### Deprecated

-

### Removed

-

### Fixed

-

### Security

-

---

## [0.5.0](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.5.0) - 2023-06-02

### Added

- Support the official `prometheus-client` crate for producing metrics
- Support exemplars when using the feature flags `exemplars-tracing` or `exemplars-tracing-opentelemetry`.
  Autometrics can now extract fields from the current span and attach them as exemplars on the
  counter and histogram metrics
- `ResultLabels` derive macro allows to specify on an enum whether variants should
  always be "ok", or "error" for the success rate metrics of functions using them. (#61)
- The `prometheus_exporter` module contains all functions related to the `prometheus-exporter` feature
- `prometheus_exporter::encode_http_response` function returns an `http::Response` with the metrics.
  This is especially recommended when using exemplars, because it automatically uses the OpenMetrics
  `Content-Type` header, which is required for Prometheus to scrape metrics with exemplars
- `AUTOMETRICS_DISABLE_DOCS` environment variable can be set to disable doc comment generation
  (this is mainly for use with editor extensions that generate doc comments themselves)

### Changed

- Users must configure the metrics library they want to use autometrics with, unless the
  `prometheus-exporter` feature is enabled. In that case, the official `prometheus-client` will be used
- Metrics library feature flags are now mutually exclusive (previously, `autometrics` would only
  produce metrics using a single metrics library if multiple feature flags were enabled, using
  a prioritization order defined internally)
- `GetLabels` trait (publicly exported but meant for internal use) changed the signature
  of its function to accomodate the new `ResultLabels` macro. This change is not significant
  if you never imported `autometrics::__private` manually (#61)
- When using the `opentelemetry` together with the `prometheus-exporter`, it will no longer
  use the default registry provided by the `prometheus` crate. It will instead use a new registry
- Updated `opentelemetry` dependencies to v0.19. This means that users using autometrics
  with `opentelemetry` but not using the `prometheus-exporter` must update the `opentelemetry`
  to use v0.19.

### Deprecated
- `global_metrics_exporter` and `encode_global_metrics` have been deprecated and replaced by
  `prometheus_exporter::init` and `prometheus_exporter::encode_to_string`, respectively

### Removed

- `opentelemetry` is no longer used by default
- `GetLabelsForResult` trait (publicly exported but meant for internal use) was removed
  to accomodate the new `ResultLabels` macro. This change is not significant
  if you never imported `autometrics::__private` manually (#61)

### Fixed

- `#[autometrics]` now works on functions that use type inference in their return statement
  (#74, #61)

## [0.4.1](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.4.1) - 2023-05-05

### Changed

- Overhaul documentation

### Fixed

- Generated latency query needs additional labels to show lines for 95th and 99th percentiles

## [0.4.0](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.4.0) - 2023-04-26

### Added

- `build_info` metric tracks software version and commit
- Queries now use `build_info` metric to correlate version info with problems

### Fixed

- Prometheus rules handle when the counter metric name has a `_total` suffix

## [0.3.3](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.3.3) - 2023-04-14

### Added

- Alerts have minimum traffic threshold of 1 request / minute

### Fixed

- Latency SLO total query in alerting rules

## [0.3.1](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.3.1) - 2023-03-21

### Added

- `custom-objective-latency` and `custom-objective-percentile` feature flags

### Changed

- Use the OpenTelemetry default histogram buckets

### Removed

- Remove the latency objective values for 150, 200, and 350 milliseconds
- `custom-objectives` feature flag

## [0.3.0](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.3.0) - 2023-03-14

### Added

- Support defining Service-Level Objectives (SLOs) in code
- CLI to generate Sloth file, which is then used to generate Prometheus alerting rules
- `#[skip_autometrics]` annotation when applying autometrics to an `impl` block
- `ok_if` and `error_if` autometrics parameters

## [0.2.4](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.2.4) - 2023-02-08

### Fixed

- Histogram buckets


## [0.2.3](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.2.3) - 2023-01-31

### Fixed

- Building of documentation on docs.rs

## [0.2.0](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.2.0) - 2023-01-31

### Added

- Support `opentelemetry` and `metrics` crates for tracking metrics
- Support applying autometrics to an `impl` block

### Changed

- Tracking function concurrency is optional

## [0.1.1](https://github.com/autometrics-dev/autometrics-rs/releases/tag/v0.1.1) - 2023-01-27

### Added

- Track concurrent requests
- Add return types as labels
- Separate function call counter
- `caller` label

### Changed

- Use OpenTelemetry metric naming conventions
