- **confirm.ts**
  - Confirms a Toss Payments transaction.
  - Validates `orderId`, `amount`, and READY status.
  - Calls Toss `payments/confirm`, then updates the order (method/status/timestamps/customer info).
  - Redirects to `/payments/success?orderId=...`; on error, redirects to `/payments/fail?...`.

- **confirm_free_order.ts**
  - Confirms a zero-amount order (no external payment).
  - Validates input, ensures amount is 0 and order is READY.
  - Marks order as DONE with timestamps and optional customer info.
  - Returns JSON with `successUrl` or failure payload.

- **upsert_offline_attendee.ts**
  - Adds offline event attendees to an order.
  - Validates `orderId`, `quantity`, and attendees array length.
  - Ensures order exists and is READY; inserts attendees tied to the order.
  - On Hasura JWT expiry, returns 401 with a fail URL; otherwise 200 or 400 JSON.

- **webhook/check_order_expired.ts**
  - Hasura-scheduled webhook to expire stale orders.
  - Reads `orderId` from payload, checks if READY and older than 15 minutes.
  - If expired, updates status to EXPIRED; returns “changed” or “unchanged”.

- **webhook/delete_order.ts**
  - Admin cleanup endpoint to delete EXPIRED orders.
  - Deletes by status filter and returns deleted rows.

- **webhook/payment_status_changed.ts**
  - Updates order status based on Toss Payments webhook payload.
  - Validates `orderId`, then sets latest `status` from payload.

- **webhook/summary_order.ts**
  - Summarizes sales across content IDs.
  - Queries products and orders, computes totals (done/canceled, with/without discount, amounts).
  - Posts a formatted summary to Slack; responds 200/400.

### Cross-cutting behavior

- **Admin Hasura client**: Most routes use `getClient` with `x-hasura-admin-secret`.
- **Order lifecycle**: READY → (confirm) → DONE; READY older than 15m → EXPIRED; webhooks can update/cancel states.
- **Validation**: Strong checks on IDs, amounts, and statuses before mutations.
- **Errors**: Return proper HTTP status with redirects (pages) or JSON (API) and include fail URLs where relevant.