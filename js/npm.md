
### What is npx?

Node package executor - execute binaries inside local node_modules/ or in npm global cache.
If binary is not present anywhere, it will download to a temp location and run it.
No need of `npm run node_modules/.bin/eslint` only `npx eslint` or `npx webpack`.

### npm configuration

npm gets its config settings from the command line, environment variables, and npmrc files.

The npm config command can be used to update and edit the contents of the user and global npmrc files.

The four relevant files are:

1. per-project config file (`/path/to/my/project/.npmrc`)
2. per-user config file (`~/.npmrc`)
3. global config file (`$PREFIX/etc/npmrc`)
4. npm builtin config file (`/path/to/npm/npmrc`)

All npm config files are an ini-formatted list of key = value parameters. Environment variables can be replaced using ${VARIABLE_NAME}. For example:

### How `npm init abc` works

The init command is transformed to a corresponding npx operation as follows:
```
npm init foo -> npx create-foo
```

e.g. `npm init react-app my-react-proj` will do
`npx create-react-app my-react-proj`

### Check who owns package/if package is available

```sh
npm onwer ls <packagename>
```

### Publishing publicly scoped packages i.e. `@myscope/packagename`

1. set scope in `.npmrc` automatically via
```sh
npm config set scope myusername
```
all subsequent packages created will have scope prepended in packagename.

2. npm login must have been done.

3. check that the `name` field in `package.json` is `@scopename/packagename`.

4. for public scoped package publishing
```sh
npm publish --access=public
```

### running npm command in a different directory.

Let's say we have following project structure
```
- client
  - app.js
  - package.json
- src
- package.json
```

We want to do a `npm install` of `client/package.json` first,

We can run npm commands using a `--prefix` flag specifying the
directory where the commands needs to be run.

e.g.
```sh
npm run --prefix client build
```




