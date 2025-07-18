# OpenTelemetry Rust

The Rust [OpenTelemetry](https://opentelemetry.io/) implementation.

[![Crates.io: opentelemetry](https://img.shields.io/crates/v/opentelemetry.svg)](https://crates.io/crates/opentelemetry)
[![LICENSE](https://img.shields.io/crates/l/opentelemetry)](./LICENSE)
[![GitHub Actions CI](https://github.com/open-telemetry/opentelemetry-rust/workflows/CI/badge.svg)](https://github.com/open-telemetry/opentelemetry-rust/actions?query=workflow%3ACI+branch%3Amain)
[![Documentation](https://docs.rs/opentelemetry/badge.svg)](https://docs.rs/opentelemetry)
[![codecov](https://codecov.io/gh/open-telemetry/opentelemetry-rust/branch/main/graph/badge.svg)](https://codecov.io/gh/open-telemetry/opentelemetry-rust)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/open-telemetry/opentelemetry-rust/badge)](https://scorecard.dev/viewer/?uri=github.com/open-telemetry/opentelemetry-rust)
[![Slack](https://img.shields.io/badge/slack-@cncf/otel/rust-brightgreen.svg?logo=slack)](https://cloud-native.slack.com/archives/C03GDP0H023)

## Overview

OpenTelemetry is a collection of tools, APIs, and SDKs used to instrument,
generate, collect, and export telemetry data (metrics, logs, and traces) for
analysis in order to understand your software's performance and behavior. You
can export and analyze them using [Prometheus], [Jaeger], and other
observability tools.

*[Supported Rust Versions](#supported-rust-versions)*

[Prometheus]: https://prometheus.io
[Jaeger]: https://www.jaegertracing.io

## Project Status

The table below summarizes the overall status of each component. Some components
include unstable features, which are documented in their respective crate
documentation.

| Signal/Component      | Overall Status     |
| --------------------  | ------------------ |
| Context               | Beta               |
| Baggage               | RC                 |
| Propagators           | Beta               |
| Logs-API              | Stable*            |
| Logs-SDK              | Stable             |
| Logs-OTLP Exporter    | RC                 |
| Logs-Appender-Tracing | Stable             |
| Metrics-API           | Stable             |
| Metrics-SDK           | Stable             |
| Metrics-OTLP Exporter | RC                 |
| Traces-API            | Beta               |
| Traces-SDK            | Beta               |
| Traces-OTLP Exporter  | Beta               |

*OpenTelemetry Rust is not introducing a new end user callable Logging API.
Instead, it provides [Logs Bridge
API](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/logs/api.md),
that allows one to write log appenders that can bridge existing logging
libraries to the OpenTelemetry log data model. The following log appenders are
available:

* [opentelemetry-appender-log](opentelemetry-appender-log/README.md)
* [opentelemetry-appender-tracing](opentelemetry-appender-tracing/README.md)

If you already use the logging APIs from above, continue to use them, and use
the appenders above to bridge the logs to OpenTelemetry. If you are using a
library not listed here, feel free to contribute a new appender for the same.

If you are starting fresh, we recommend using
[tracing](https://github.com/tokio-rs/tracing) as your logging API. It supports
structured logging and is actively maintained. `OpenTelemetry` itself uses
`tracing` for its internal logging.

Project versioning information and stability guarantees can be found
[here](VERSIONING.md).

## Getting Started

If you are new to OpenTelemetry, start with the [Stdout
Example](./opentelemetry-stdout/examples/basic.rs). This example demonstrates
how to use OpenTelemetry for logs, metrics, and traces, and display
telemetry data on your console.

For those using OTLP, the recommended OpenTelemetry Exporter for production
scenarios, refer to the [OTLP Example -
HTTP](./opentelemetry-otlp/examples/basic-otlp-http/README.md) and the [OTLP
Example - gRPC](./opentelemetry-otlp/examples/basic-otlp/README.md).

Additional examples for various integration patterns can be found in the
[examples](./examples) directory.

## Overview of crates

The following crates are maintained in this repo:

* [`opentelemetry`] This is the OpenTelemetry API crate, and is the crate
  required to instrument libraries and applications. It contains Context API,
  Baggage API, Propagators API, Logging Bridge API, Metrics API, and Tracing
  API.
* [`opentelemetry-sdk`] This is the OpenTelemetry SDK crate, and contains the
  official OpenTelemetry SDK implementation. It contains Logging SDK, Metrics
  SDK, and Tracing SDK. It also contains propagator implementations.
* [`opentelemetry-otlp`] - exporter to send telemetry (logs, metrics and traces)
  in the [OTLP
  format](https://github.com/open-telemetry/opentelemetry-specification/tree/main/specification/protocol)
  to an endpoint accepting OTLP. This could be the [OTel
  Collector](https://github.com/open-telemetry/opentelemetry-collector),
  telemetry backends like [Jaeger](https://www.jaegertracing.io/),
  [Prometheus](https://prometheus.io/docs/prometheus/latest/feature_flags/#otlp-receiver)
  or [vendor specific endpoints](https://opentelemetry.io/ecosystem/vendors/).
* [`opentelemetry-stdout`] exporter for sending logs, metrics and traces to
  stdout, for learning/debugging purposes.  
* [`opentelemetry-http`] This crate contains utility functions to help with
  exporting telemetry, propagation, over [`http`].
* [`opentelemetry-appender-log`] This crate provides logging appender to route
  logs emitted using the [log](https://docs.rs/log/latest/log/) crate to
  opentelemetry.
* [`opentelemetry-appender-tracing`] This crate provides logging appender to
  route logs emitted using the [tracing](https://crates.io/crates/tracing) crate
  to opentelemetry.  
* [`opentelemetry-jaeger-propagator`] provides context propagation using [jaeger
  propagation
  format](https://www.jaegertracing.io/docs/1.18/client-libraries/#propagation-format).
* [`opentelemetry-prometheus`] provides a pipeline and exporter for sending
  metrics to [`Prometheus`].
* [`opentelemetry-semantic-conventions`] provides standard names and semantic
  otel conventions.
* [`opentelemetry-zipkin`] provides a pipeline and exporter for sending traces
  to [`Zipkin`].

In addition, there are several other useful crates in the [OTel Rust Contrib
repo](https://github.com/open-telemetry/opentelemetry-rust-contrib). A lot of
crates maintained outside OpenTelemetry owned repos can be found in the
[OpenTelemetry
Registry](https://opentelemetry.io/ecosystem/registry/?language=rust).

[`opentelemetry`]: https://crates.io/crates/opentelemetry
[`opentelemetry-sdk`]: https://crates.io/crates/opentelemetry-sdk
[`opentelemetry-appender-log`]: https://crates.io/crates/opentelemetry-appender-log
[`opentelemetry-appender-tracing`]: https://crates.io/crates/opentelemetry-appender-tracing
[`opentelemetry-http`]: https://crates.io/crates/opentelemetry-http
[`opentelemetry-otlp`]: https://crates.io/crates/opentelemetry-otlp
[`opentelemetry-stdout`]: https://crates.io/crates/opentelemetry-stdout
[`opentelemetry-jaeger-propagator`]: https://crates.io/crates/opentelemetry-jaeger-propagator
[`opentelemetry-prometheus`]: https://crates.io/crates/opentelemetry-prometheus
[`Prometheus`]: https://prometheus.io
[`opentelemetry-zipkin`]: https://crates.io/crates/opentelemetry-zipkin
[`Zipkin`]: https://zipkin.io
[`opentelemetry-semantic-conventions`]: https://crates.io/crates/opentelemetry-semantic-conventions
[`http`]: https://crates.io/crates/http

## Supported Rust Versions

OpenTelemetry is built against the latest stable release. The minimum supported
version is 1.75. The current OpenTelemetry version is not guaranteed to build
on Rust versions earlier than the minimum supported version.

The current stable Rust compiler and the three most recent minor versions
before it will always be supported. For example, if the current stable compiler
version is 1.49, the minimum supported version will not be increased past 1.46,
three minor versions prior. Increasing the minimum supported compiler version
is not considered a semver breaking change as long as doing so complies with
this policy.

## Contributing

See the [contributing file](CONTRIBUTING.md).

The Rust special interest group (SIG) meets weekly on Tuesdays at 9 AM Pacific
Time. The meeting is subject to change depending on contributors' availability.
Check the [OpenTelemetry community
calendar](https://github.com/open-telemetry/community?tab=readme-ov-file#calendar)
for specific dates and for Zoom meeting links. "OTel Rust SIG" is the name of
meeting for this group.

Meeting notes are available as a public [Google
doc](https://docs.google.com/document/d/12upOzNk8c3SFTjsL6IRohCWMgzLKoknSCOOdMakbWo4/edit).
If you have trouble accessing the doc, please get in touch on
[Slack](https://cloud-native.slack.com/archives/C03GDP0H023).

The meeting is open for all to join. We invite everyone to join our meeting,
regardless of your experience level. Whether you're a seasoned OpenTelemetry
developer, just starting your journey, or simply curious about the work we do,
you're more than welcome to participate!

## Approvers and Maintainers

### Maintainers

* [Cijo Thomas](https://github.com/cijothomas), Microsoft
* [Harold Dost](https://github.com/hdost)
* [Lalit Kumar Bhasin](https://github.com/lalitb), Microsoft
* [Utkarsh Umesan Pillai](https://github.com/utpilla), Microsoft
* [Zhongyang Wu](https://github.com/TommyCpp)

For more information about the maintainer role, see the [community repository](https://github.com/open-telemetry/community/blob/main/guides/contributor/membership.md#maintainer).

### Approvers

* [Anton Grübel](https://github.com/gruebel), Baz
* [Björn Antonsson](https://github.com/bantonsson), Datadog
* [Scott Gerring](https://github.com/scottgerring), Datadog
* [Shaun Cox](https://github.com/shaun-cox), Microsoft

For more information about the approver role, see the [community repository](https://github.com/open-telemetry/community/blob/main/guides/contributor/membership.md#approver).

### Emeritus

* [Dirkjan Ochtman](https://github.com/djc)
* [Isobel Redelmeier](https://github.com/iredelmeier)
* [Jan Kühle](https://github.com/frigus02)
* [Julian Tescher](https://github.com/jtescher)
* [Mike Goldsmith](https://github.com/MikeGoldsmith)

For more information about the emeritus role, see the [community repository](https://github.com/open-telemetry/community/blob/main/guides/contributor/membership.md#emeritus-maintainerapprovertriager).

### Thanks to all the people who have contributed

[![contributors](https://contributors-img.web.app/image?repo=open-telemetry/opentelemetry-rust)](https://github.com/open-telemetry/opentelemetry-rust/graphs/contributors)
