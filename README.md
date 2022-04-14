# Minimal repro for issue with nx running local plugins

Steps to reproduce:

- run `yarn`
- run `yarn nx g @my-workspace/my-plugin:app testapp`

# Issue

<!-- Please do your best to fill out all of the sections below! -->

## Current Behavior

<!-- What is the behavior that currently you experience? -->

Using a local plugin generator fails with error "Cannot find module", as shown below.

## Expected Behavior

<!-- What is the behavior that you expect to happen? -->
<!-- Is this a regression? .i.e Did this used to be the behavior at one point?  -->

As written in the [docs](https://nx.dev/nx-plugin/overview#using-your-nx-plugin), a plugin should be able to be used locally.

## Steps to Reproduce

<!-- Help us help you by making it easy for us to reproduce your issue! -->

<!-- Can you reproduce this on https://github.com/nrwl/nx-examples? -->
<!-- If so, open a PR with your changes and link it below. -->
<!-- If not, please provide a minimal Github repo -->
<!-- At the very least, provide as much detail as possible to help us reproduce the issue -->

<!-- Remove this line -->

Run the following commands to create a new nx workspace with a local plugin package. `yarn` has been used, but the same result is obtained with `npm`.

```sh
npx create-nx-workspace@latest --preset=core my-workspace
cd my-workspace
rm -r node_modules
rm package-lock.json
echo '{"version": 2, "projects": {}}' >> workspace.json
yarn add -D @nrwl/nx-plugin -W
yarn nx g @nrwl/nx-plugin:plugin my-plugin --importPath @my-workspace/my-plugin
yarn nx g @nrwl/nx-plugin:generator app --project my-plugin
yarn nx g @my-workspace/my-plugin:app testapp --verbose
```

The last command fails with the error specified in [Failure Logs](#failure-logs).

As a workaround, the workspace files can be removed from the `node_modules` folder. With the additional following commands, the generator works as expected:

```sh
rm -r node_modules/@my-workspace/my-plugin
yarn nx g @my-workspace/my-plugin:app testapp
```

### Failure Logs

<!-- Please include any relevant log snippets or files here. -->

```
...  my-workspace % yarn nx g @my-workspace/my-plugin:app testapp --verbose

yarn run v1.22.15
$ /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/.bin/nx g @my-workspace/my-plugin:app testapp --verbose
Cannot find module '/Users/davide/dev/temp/nx-issue/my-workspace/packages/my-plugin/src/generators/app/generator'
Require stack:
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/src/config/workspaces.js
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/src/command-line/generate.js
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/src/command-line/nx-commands.js
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/bin/init-local.js
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/bin/nx.js
Error: Cannot find module '/Users/davide/dev/temp/nx-issue/my-workspace/packages/my-plugin/src/generators/app/generator'
Require stack:
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/src/config/workspaces.js
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/src/command-line/generate.js
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/src/command-line/nx-commands.js
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/bin/init-local.js
- /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/bin/nx.js
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:933:15)
    at Function.Module._load (node:internal/modules/cjs/loader:778:27)
    at Module.require (node:internal/modules/cjs/loader:999:19)
    at require (/Users/davide/dev/temp/nx-issue/my-workspace/node_modules/v8-compile-cache/v8-compile-cache.js:159:20)
    at /Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/src/config/workspaces.js:122:28
    at Object.<anonymous> (/Users/davide/dev/temp/nx-issue/my-workspace/node_modules/nx/src/command-line/generate.js:127:40)
    at Generator.next (<anonymous>)
    at fulfilled (/Users/davide/dev/temp/nx-issue/my-workspace/node_modules/tslib/tslib.js:114:62)
error Command failed with exit code 1.
```

### Environment

<!-- It's important for us to know the context in which you experience this behavior! -->
<!-- Please paste the result of `nx report` below! -->

```
   Node : 17.8.0
   OS   : darwin arm64
   yarn : 1.22.15

   nx : 13.10.1
   @nrwl/angular : Not Found
   @nrwl/cypress : Not Found
   @nrwl/detox : Not Found
   @nrwl/devkit : 13.10.1
   @nrwl/eslint-plugin-nx : 13.10.1
   @nrwl/express : Not Found
   @nrwl/jest : 13.10.1
   @nrwl/js : 13.10.1
   @nrwl/linter : 13.10.1
   @nrwl/nest : Not Found
   @nrwl/next : Not Found
   @nrwl/node : Not Found
   @nrwl/nx-cloud : Not Found
   @nrwl/nx-plugin : 13.10.1
   @nrwl/react : Not Found
   @nrwl/react-native : Not Found
   @nrwl/schematics : Not Found
   @nrwl/storybook : Not Found
   @nrwl/web : Not Found
   @nrwl/workspace : 13.10.1
   typescript : 4.6.3
   rxjs : 6.6.7
   ---------------------------------------
```
