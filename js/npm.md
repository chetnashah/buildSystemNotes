
### What is npx?

Node package executor - execute binaries inside local node_modules/ or in npm global cache.
If binary is not present anywhere, it will download to a temp location and run it.
No need of `npm run node_modules/.bin/eslint` only `npx eslint` or `npx webpack`.