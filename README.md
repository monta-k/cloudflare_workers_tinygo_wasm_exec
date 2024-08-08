# cloudflare_workers_tinygo_wasm_exec

- `cloudflare_workers_tinygo_wasm_exec` is a package to run run a TinyGo WASM on Cloudflare Workers with `compatibility_flags = ["nodejs_compat"]`.

## Differences from TinyGo's `wasm_exec.js`

- To avoid the `Uncaught RangeError: Maximum call stack size exceeded` error, some code has been removed.

```
if (!global.fs && global.require) {
		global.fs = require("fs");
	}
```

https://github.com/tinygo-org/tinygo/blob/dd6fa89aa66a5113baa8883d4180ee090f35f784/targets/wasm_exec.js#L31-L33

```
if (
		global.require &&
		global.require.main === module &&
		global.process &&
		global.process.versions &&
		!global.process.versions.electron
	) {
		if (process.argv.length != 3) {
			console.error("usage: go_js_wasm_exec [wasm binary] [arguments]");
			process.exit(1);
		}

		const go = new Go();
		WebAssembly.instantiate(fs.readFileSync(process.argv[2]), go.importObject).then((result) => {
			return go.run(result.instance);
		}).catch((err) => {
			console.error(err);
			process.exit(1);
		});
	}
```

https://github.com/tinygo-org/tinygo/blob/dd6fa89aa66a5113baa8883d4180ee090f35f784/targets/wasm_exec.js#L507-L526

- Updated the wasm_exec.js file to include the node: prefix for all Node.js API imports.

```javascript
...
const nodeCrypto = require("node:crypto");
...
global.TextEncoder = require("node:util").TextEncoder;
...
```

## License

This project uses code from the [TinyGo](https://github.com/tinygo-org/tinygo) repository, which is licensed under the BSD 3-Clause License. Please refer to the TinyGo repository for more details.
