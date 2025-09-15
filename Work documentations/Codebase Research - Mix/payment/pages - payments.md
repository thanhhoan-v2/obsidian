- **start.tsx**
  - Entry point to start an order for a product (`content_id`, `product_id`, optional `discount_apply`).
  - Server-side flow:
    - Loads current user (if any) via `SessionDocument`.
    - Validates product via admin client (`ProductByPkDocument`), checking `on_order` and discount rules.
    - Reuses an existing READY order for the same product and price if an `order_token` cookie matches; otherwise creates a new temp order with a fresh UUID token.
    - Creates a Hasura scheduled event to expire the temp order in 1 hour.
    - Sets `order_token` cookie and redirects to `/payments/form/[orderId]`.
  - On failure, renders a simple error page with a “go to product” action.

- **form/[orderId].tsx**
  - Shows the actual payment form for a specific order.
  - Server-side flow:
    - Requires valid `order_token` cookie and a string `orderId`.
    - Loads session to determine `me` (logged-in vs guest).
    - Fetches the order; validates:
      - `order.id` exists
      - `order.order_token` matches cookie
      - `order.customer_id` matches `me?.id` (or null for guest)
      - `order.status` is READY
    - On success, passes the order to the page which renders either `PaymentForm` or `PaymentFormPostForum` based on title.
    - On any validation error, redirects to `/payments/fail?orderId=...&message=...`.

- **success.tsx**
  - Displays the order confirmation.
  - Server-side flow:
    - If `order_token` cookie is missing, treats session as expired and shows a minimal “expired” success state with only the `orderId`.
    - Otherwise fetches the order and ensures `status === Done`.
    - Attempts to fetch payment details from Toss Payments API for receipt link.
  - Client-side:
    - Fires analytics (GA, FB pixel) on successful load.
    - Shows order details, total, optional method, and “영수증 보기” button if available.
    - Buttons to return home or view order history (for logged-in users).

- **fail.tsx**
  - Displays a user-friendly failure page.
  - Server-side flow:
    - Reads optional `message`, `code`, `orderId` from query.
    - If `orderId` provided, tries to fetch the order and compute a “다시 주문하기” `redirectUrl` back to the product page.
  - Client-side:
    - Shows failure info and buttons to go home or retry ordering (when `redirectUrl` exists).

### Cross-cutting details

- **Authentication context**: Uses `getClient` with either user cookies or admin headers depending on the operation (user/session checks vs product/order creation).
- **Order session**: Managed via `order_token` httpOnly cookie; ensures continuity and validity across pages.
- **Error routing**: Any validation or unexpected error redirects to `/payments/fail` with query parameters.
- **Scheduling**: On temp order creation, a Hasura scheduled event is created to expire incomplete orders after 1 hour.
- **Analytics**: Success page emits GA and FB events with order params.

This directory orchestrates the full order lifecycle: validate and create (start) → collect details and pay (form) → show result (success/fail).