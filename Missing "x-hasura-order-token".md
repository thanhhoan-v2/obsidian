## The problem

1. User visits `/payments/start?content_id=X&product_id=Y`
2. `getServerSideProps` calls `checkProduct()` (line 155)
3. `checkProduct()` calls `getAdminClient()` (line 228)
4. GraphQL query fails because "x-hasura-order-token" header is missing
5. Error: "missing session variable: "x-hasura-order-token""

## The fix

The `getAdminClient()` function needs to optionally accept an order token parameter. The issue is that `checkProduct()` is called before we have an order token, so we need to:

Update `getAdminClient()` to accept optional order token
```js
export const getAdminClient = (orderToken?: string) => {
	const headers: any = {
		"x-hasura-admin-secret": process.env.HASURA_ADMIN_SECRET,
	};

	if (orderToken) {
		headers["x-hasura-order-token"] = orderToken;
	}

	return getClient({
		headers,
		ctx: null,
	});
};
```

Update `checkProduct()` to accept order token
```js
async function checkProduct(..., orderToken?: string) {
	const admin = getAdminClient(orderToken);
	...
}
```

Move **order token creation** before **product validation**

BEFORE
![[Screenshot 2025-09-15 at 10.24.30.png]]

AFTER
![[Screenshot 2025-09-15 at 10.23.53.png]]


