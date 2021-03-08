# Repro

## Instructions

```
pnpm i
cd foo/bar
pnpm add asdfghjkl11
```

```
 ERROR  GET https://registry.npmjs.org/asdfghjkl11: Not Found - 404
asdfghjkl11 is not in the npm registry, or you have no permission to fetch it.

An authorization header was used: Bearer 8597[hidden]
```

=> `packages/foo/bar/node_modules` is created containing `pnpm.debug.log`.

This should not happen.

Then when you run, `pnpm add lodash`, `packages/foo/bar/package.json` is created because it find the closest `node_modules` dir.

_packages/foo/bar/package.json_

```
{
	"dependencies": {
		"lodash": "^4.17.21"
	}
}
```
