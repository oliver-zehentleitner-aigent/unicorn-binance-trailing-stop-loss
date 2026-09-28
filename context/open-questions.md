# Open questions

Found during a retrospective pass (2026-07) — genuinely unknown, not resolved by code, commit history, or existing docs. Flagging rather than guessing (per Keep the Why rule 1). The second one, on `jump-in-and-trail`, was answered by the maintainer on 2026-09-28 and moved to `engines.md`.

## Uncapped retry loop on Binance error `-2010`

**Id:** 0ee4ef2c-e5b1-4c08-9cc6-622f82870797
**Type:** undefined — open question awaiting maintainer input, not yet classifiable as decision/workaround/incident/constraint
**Status:** open
**Evidence:** unknown
**Source:** retrospective pass, 2026-07; maintainer, 2026-09-28 — the original reason is not known any more
**See:** fail-loud.md#update_stop_loss_asset_amount-fails-loud-instead-of-crashing-silently-deep-in-the-engine-thread — a89d2f74-2078-411c-bf32-a36a33cfcec2 — as of 2026-09-28

`manager.py` (~lines 573-580), inside `create_stop_loss_order`: on Binance error code `-2010`, the code does `time.sleep(5)` and loops (`while order_is_placed is False`) with no retry cap — indefinite retry on a fixed 5-second interval. Other error codes `return False` immediately instead of retrying.

**Why this needs an answer:** if `-2010` ("insufficient balance" in Binance's error scheme, among other cases) can also fire for a *permanent* condition, not just a transient one, this retries forever rather than failing loud — which would be inconsistent with the fail-loud pattern documented in `fail-loud.md`. Unknown whether `-2010` is scoped narrowly enough here that infinite retry is actually safe, or whether this needs a cap.

**Asked, 2026-09-28:** the original reason for the uncapped retry is not recoverable — the maintainer no longer knows it, and nobody else does. What is left is a decision for the future, not a lost fact to find: keep the loop, cap it, or treat the `-2010` cases apart. One plausible purpose is bridging the moment after the previous stop-loss order is cancelled, while the asset is still locked; but `-2010` also covers an order that would trigger immediately — the price already below the stop — and there the loop would retry for as long as the price stays there, with no stop-loss order in place.

