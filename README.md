This repository reproduces an issue with @percy/cli and @percy/storybook when using pnpm workspace with the option `hoist=false` in the `.npmrc`.

For context `hoist=false` will scope all dependencies only to the packages that reference it. Meaning, if a determined package import dependencies not declared in it's own dependencies it will fail.

- In order to see the error please run `pnpm install`
- Then run `pnpm run --filter a visual-test`
- This command will run the script called `visual-test` in the file packages/a/package.json. 
- The expected behaviour running this command should be: `[percy] Error: Not found: ./storybook-static`
- Instead the result will be `[percy] Error: Cannot find package '@percy/core' imported from /Users/myuser/projects/percy/percy-storybook-dep-issue/node_modules/.pnpm/@percy+storybook@5.0.1/node_modules/@percy/storybook/dist/config.js`

The reason for the error is because [src/config.json](https://github.com/percy/percy-storybook/blob/596228e9a9b75ae2f7e15e334300eb40a720507d/src/config.js#L1C33-L1C51) imports `snapshotSchema` from `@percy/core/config` but `@percy/core` is not listed as a dependency of [package.json](https://github.com/percy/percy-storybook/blob/master/package.json#L41C20-L41C20).