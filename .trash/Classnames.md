---
1-day revision: false
---
1. Create classes[]
2. For each arg in args:
	- if arg is falsy: return
	- if arg is string or number: classes.push(arg) & return
	- if arg is array: for each item in arg: classes.push(item) & return
	- if arg is object: for each key in arg: if arg[key] is true: classes.push(key) & return
3. Join classes[] with a space

```js
/**
 * @param {...(any|Object|Array<any|Object|Array>)} args
 * @return {string}
 */

export default function classNames(...args) {
	const classes = [];

	args.forEach((arg) => {
		if (!arg) return;

		const argType = typeof arg;

		if (argType === "string" || argType === "number") {
			classes.push(arg);
			return;
		}

		if (Array.isArray(arg)) {
			classes.push(classNames(...arg));
			return;
		}

		if (argType === "object") {
			for (const key in arg) {
				if (Object.hasOwn(arg, key) && arg[key]) {
					classes.push(key);
				}
			}
			return;
		}
	});

	return classes.join(" ");
}
```