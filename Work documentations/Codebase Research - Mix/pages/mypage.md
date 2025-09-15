### /mypage (pages/mypage/index.tsx)
- **Purpose**: Profile page to view/edit user info.
- **Auth**: SSR checks session; unauthenticated users redirected to `/#signin`.
- **UI**: Shows name, login method/email, selects for position/career, toggle for marketing emails.
- **Edit flow**: “수정하기/저장하기” toggles edit mode; on save, runs `useUpdateMeMutation` then reloads.
- **Tech**: Uses `useAuth` for `me`, Emotion for styling, and hydrates Apollo via `__APOLLO_CACHE_DATA__`.

### /mypage/courses (pages/mypage/courses/index.tsx)
- **Purpose**: “내 강의실” listing purchased/available courses.
- **Auth**: SSR session guard; redirects to `/#signin` if not logged in.
- **Data**: SSR fetches `MixClassCourseDocument` and provides as `courses`; client also calls `useMixClassCourseQuery` (cache-and-network) and merges with SSR.
- **UI**: Renders `CourseList courses={courses}` under `PageTitle`.

### /mypage/orders (pages/mypage/orders/index.tsx)
- **Purpose**: Order history.
- **Auth**: SSR session guard; redirects to `/#signin`.
- **Data**: Filters by `customer_id = me.id` and `status ∈ {Done, WaitingForDeposit, WaitingForCanceled, Canceled, PartialCanceled}`. SSR provides `orders`; client re-fetches with `useOrderQuery`.
- **UI**: `OrderList orders={orders}` under `PageTitle`.

### /mypage/orders/`[id]` (pages/mypage/orders/`[id]`.tsx)
- **Purpose**: Order detail page with payment info.
- **Auth**: SSR session guard; redirects to `/#signin`.
- **Data**:
  - SSR loads `order_by_pk` by `id`; on failure redirects to `/mypage/orders`.
  - Queries Toss Payments REST API for payment info using `TOSS_SECRET_KEY` (Basic auth) and includes it if available.
  - Client re-fetches order via `useOrderByPkQuery`; uses SSR data as fallback.
- **UI**: `PageTitle` with back icon to orders list, `OrderDetail order={order} payment={payment}`.

- All pages construct Apollo via `getClient`, pass `__APOLLO_CACHE_DATA__` for hydration, and rely on `useAuth` for client context.