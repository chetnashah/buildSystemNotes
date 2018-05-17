
### What is npx?

Node package executor - execute binaries inside local node_modules/ or in npm global cache.
If binary is not present anywhere, it will download to a temp location and run it.
No need of `npm run node_modules/.bin/eslint` only `npx eslint` or `npx webpack`.

### npm configuration

npm gets its config settings from the command line, environment variables, and npmrc files.

The npm config command can be used to update and edit the contents of the user and global npmrc files.

The four relevant files are:

1. per-project config file (/path/to/my/project/.npmrc)
2. per-user config file (~/.npmrc)
3. global config file ($PREFIX/etc/npmrc)
4. npm builtin config file (/path/to/npm/npmrc)

All npm config files are an ini-formatted list of key = value parameters. Environment variables can be replaced using ${VARIABLE_NAME}. For example:

