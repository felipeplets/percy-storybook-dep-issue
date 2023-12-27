This repository reproduces an issue with @percy/cli and @percy/storybook when using pnpm workspace.

- In order to see the error please run `pnpm install`
- Then run `pnpm run --filter a visual-test`
- This command will run the script called `visual-test` in the file package.json. 
- The expected behaviour running this command should be: `[percy] Error: Not found: ./storybook-static`
- Instead the result will be `[percy] ParseError: Unexpected argument 'storybook'`

The reason for the error is because the `importCommands` function in `packages/cli/src/commands.js` will find the following folder as its sibling `./node_modules/.pnpm/@percy+cli@1.27.6/node_modules/@percy` which only contain symlinks to `@percy/cli` dependencies. As other commands packages are not dependencies this breaks compatibility. In order to keep it working as expected the folder `./node_modules` would need to be added.