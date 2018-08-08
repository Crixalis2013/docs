# Benchmark Results Schema

* Updated: 2018 August 9

[TOC]

This document describes the JSON schema that Fuchsia benchmark results must follow in
order to be uploaded to the performance dashboard.

## JSON Description

```json
[
    {
        "label":       string     // Name of the test case in the performance dashboard.
        "test_suite":  string     // Name of the test suite in the performance dashboard.
        "unit":        string     // One of the supported units (see below)
        "values":      [v1, v2..] // Numeric values collected in this test case
        "split_first": bool       // Whether to split the first element in |values| from the rest.
    },
    {
        ...
    }
]
```

## Supported Units:

In order to convert benchmark results to the format required by the performance
dashboard, `unit` must be one of the following strings, which desribe the units
of the result's `values`.

* `nanoseconds`  or `ns`
* `milliseconds` or `ms`
* `bytes/second`

### Example

```json
[
    {
        "label": "Channel/WriteRead/64bytes",
        "test_suite": "fuchsia.zircon_benchmarks",
        "unit": "nanoseconds",
        "values": [105.45, 697.916667, 672.743056],
        "split_first": true
    },
    {
        "label":"Channel/WriteRead/1024bytes",
        "test_suite":"fuchsia.zircon_benchmarks",
        "unit":"nanoseconds",
        "values":[102.23, 1004.340278, 906.250000],
        "split_first": true
    }
]
```

## split_first behavior

split_first is useful when the first value in the test results is usually skewed
due to external influence on the test (e.g. empty caches).  When true, benchmark
results will appear as two separate series in the performance dashboard:

1. `$label/samples_0_to_0` which tracks the first element in Values, and
1. `$label/samples_1_to_N` which tracks the remaining values.

## Implementation

Rather than write code to produce this JSON yourself, you can use one of the
existing Fuchsia libraries for your language:

* [C/C++]
* [Go]
* [Dart] Dart libraries are WIP


[C/C++]: https://fuchsia.googlesource.com/zircon/+/master/system/ulib/perftest
[Go]: https://fuchsia.googlesource.com/garnet/+/master/go/src/benchmarking
[Dart]: #