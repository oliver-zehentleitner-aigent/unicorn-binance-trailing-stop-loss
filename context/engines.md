# Engines

The engine modes of the trailing stop loss manager — what each one covers and why its scope is what it is.

## `jump-in-and-trail` exists for isolated margin only — built and tested there, not excluded elsewhere

**Id:** 18ccb9bd-f183-4fb7-afdb-3cee6b0cc8e6
**Type:** constraint
**Status:** active
**Evidence:** confirmed
**Source:** maintainer, 2026-09-28; README and `meta.yaml` ("still experimental, only available for Isolated Margin"); commits `d1ef241`, `6b0942e`, `acb2c88`
**Revisit when:** support for spot, futures or cross margin is asked for or built

The `jump-in-and-trail` engine — buy in, then trail — runs only on `binance.com-isolated_margin`. Every other exchange stops with "not supported" (`manager.py`, the `jump-in-and-trail` branch of `start()`).

**Reason:** it was built for isolated margin and has only ever been tested there. Spot, futures and cross margin were not tried and rejected; nothing in the design rules them out. The isolated-margin branch itself is unfinished — the `borrow_threshold` loan and the average buy price are still TODOs in the code — which is part of why the mode is labelled experimental.

**Rejected alternative:** none on record. No other exchange was in contention; extending the mode would be new work, not the reversal of a decision.
