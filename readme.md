# Repro

## Instructions

### Part 1

```
pnpm i
cd packages/foo/bar
```

Add a package that is not in registry:

```
pnpm add asdfghjkl11
```

Note error message:

```
 ERROR  GET https://registry.npmjs.org/asdfghjkl11: Not Found - 404
asdfghjkl11 is not in the npm registry, or you have no permission to fetch it.

An authorization header was used: Bearer 8597[hidden]
```

**Actual:** `packages/foo/bar/node_modules` is created containing `pnpm.debug.log`.

**Expected:** This should not happen.

### Part 2

From same dir, run `pnpm add lodash`

**Actual:** `packages/foo/bar/package.json` is created because it finds the closest `node_modules` dir and assumes this is the package root.

_packages/foo/bar/package.json_

```
{
	"dependencies": {
		"lodash": "^4.17.21"
	}
}
```

**Expected:** If there is a `node_modules` dir, but not a `package.json`, it should continue looking for a `package.json` upwards.

## Problems

This can cause very hard to find errors. If you use `package.json#babel` to define presets for package transpilation, because it finds a spurious `package.json`, it will ignore the real package.json file, and you will get errors that you can't transpile.

Tons of other problems with any tooling that references `package.json` files relative to the file being processed.
