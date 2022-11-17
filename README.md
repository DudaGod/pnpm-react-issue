# pnpm-react-issue

Example with incorrect resolving @types/react in d.ts files of react component libraries. Problem reproduced when virtual store directory is pointed to some directory in home.

### How to reproduce problem

1. Install deps - `pnpm i`
2. Run `pnpm run build` to build react component and got error like:
```
src/Component.tsx:4:17 - error TS2786: 'Button' cannot be used as a JSX component.
  Its instance type 'Button' is not a valid JSX element.
    Type 'Button' is missing the following properties from type 'ElementClass': render, context, setState, forceUpdate, and 3 more.

4 export default <Button>Text</Button>;
                  ~~~~~~


Found 1 error in src/Component.tsx:4
```

If run `tsc` with `traceResolution: true` then I get log which shows a problem. Piece of log with correct resolve `react` deps inside my component:
```
======== Resolving module 'react' from '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/src/Component.tsx'. ========
Module resolution kind is not specified, using 'NodeJs'.
Loading module 'react' from 'node_modules' folder, target file type 'TypeScript'.
Directory '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/src/node_modules' does not exist, skipping all lookups in it.
Found 'package.json' at '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/package.json'.
'package.json' does not have a 'typesVersions' field.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react.ts' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react.tsx' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react.d.ts' does not exist.
'package.json' does not have a 'typings' field.
'package.json' does not have a 'types' field.
'package.json' has 'main' field 'index.js' that references '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js'.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js' exist - use it as a name resolution result.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js' has an unsupported extension, so skipping it.
Loading module as file / folder, candidate module location '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js', target file type 'TypeScript'.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js.ts' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js.tsx' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js.d.ts' does not exist.
File name '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js' has a '.js' extension - stripping it.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.ts' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.tsx' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.d.ts' does not exist.
Directory '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js' does not exist, skipping all lookups in it.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.ts' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.tsx' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.d.ts' does not exist.
Found 'package.json' at '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react/package.json'.
'package.json' does not have a 'typesVersions' field.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react.d.ts' does not exist.
'package.json' does not have a 'typings' field.
'package.json' has 'types' field 'index.d.ts' that references '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react/index.d.ts'.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react/index.d.ts' exist - use it as a name resolution result.
Resolving real path for '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react/index.d.ts', result '/Users/dudkevich/.pnpm-react-issue/pnpm-virtual-store/@types+react@17.0.52/node_modules/@types/react/index.d.ts'.
======== Module name 'react' was successfully resolved to '/Users/dudkevich/.pnpm-react-issue/pnpm-virtual-store/@types+react@17.0.52/node_modules/@types/react/index.d.ts' with Package ID '@types/react/index.d.ts@17.0.52'. ========
// ...
```

And when try to resolve `react` inside `Button` component from `semantic-ui` (if look log below):
```
// ...
======== Resolving module 'react' from '/Users/dudkevich/.pnpm-react-issue/pnpm-virtual-store/semantic-ui-react@2.1.3_sfoxds7t5ydpegc3knd667wn6m/node_modules/semantic-ui-react/dist/commonjs/elements/Button/Button.d.ts'. ========
Module resolution kind is not specified, using 'NodeJs'.
Loading module 'react' from 'node_modules' folder, target file type 'TypeScript'.
Directory '/Users/dudkevich/.pnpm-react-issue/pnpm-virtual-store/semantic-ui-react@2.1.3_sfoxds7t5ydpegc3knd667wn6m/node_modules/semantic-ui-react/dist/commonjs/elements/Button/node_modules' does not exist, skipping all lookups in it.
Directory '/Users/dudkevich/.pnpm-react-issue/pnpm-virtual-store/semantic-ui-react@2.1.3_sfoxds7t5ydpegc3knd667wn6m/node_modules/semantic-ui-react/dist/commonjs/elements/node_modules' does not exist, skipping all lookups in it.
Resolution for module 'react' was found in cache from location '/Users/dudkevich/.pnpm-react-issue/pnpm-virtual-store/semantic-ui-react@2.1.3_sfoxds7t5ydpegc3knd667wn6m/node_modules/semantic-ui-react/dist/commonjs'.
======== Module name 'react' was successfully resolved to '/Users/dudkevich/.pnpm-react-issue/pnpm-virtual-store/react@17.0.2/node_modules/react/index.js' with Package ID 'react/index.js@17.0.2'. ========
// ...
```

And there react resolves to `react/index.js@17.0.2` instead `@types/react/index.d.ts@17.0.52` and this gives an error described above.

### How it works when virtual-store-dir is not set

Piece of log with correct resolve `react` deps inside my component:
```
======== Resolving module 'react' from '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/src/Component.tsx'. ========
Module resolution kind is not specified, using 'NodeJs'.
Loading module 'react' from 'node_modules' folder, target file type 'TypeScript'.
Directory '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/src/node_modules' does not exist, skipping all lookups in it.
Found 'package.json' at '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/package.json'.
'package.json' does not have a 'typesVersions' field.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react.ts' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react.tsx' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react.d.ts' does not exist.
'package.json' does not have a 'typings' field.
'package.json' does not have a 'types' field.
'package.json' has 'main' field 'index.js' that references '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js'.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js' exist - use it as a name resolution result.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js' has an unsupported extension, so skipping it.
Loading module as file / folder, candidate module location '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js', target file type 'TypeScript'.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js.ts' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js.tsx' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js.d.ts' does not exist.
File name '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js' has a '.js' extension - stripping it.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.ts' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.tsx' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.d.ts' does not exist.
Directory '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.js' does not exist, skipping all lookups in it.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.ts' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.tsx' does not exist.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/react/index.d.ts' does not exist.
Found 'package.json' at '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react/package.json'.
'package.json' does not have a 'typesVersions' field.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react.d.ts' does not exist.
'package.json' does not have a 'typings' field.
'package.json' has 'types' field 'index.d.ts' that references '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react/index.d.ts'.
File '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react/index.d.ts' exist - use it as a name resolution result.
Resolving real path for '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/@types/react/index.d.ts', result '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/.pnpm/@types+react@17.0.52/node_modules/@types/react/index.d.ts'.
======== Module name 'react' was successfully resolved to '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/.pnpm/@types+react@17.0.52/node_modules/@types/react/index.d.ts' with Package ID '@types/react/index.d.ts@17.0.52'. ========
// ...
```

Piece of log with resolve `react` inside `Button` component from `semantic-ui` (if look log below):
```
// ...
======== Resolving module 'react' from '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/.pnpm/semantic-ui-react@2.1.3_sfoxds7t5ydpegc3knd667wn6m/node_modules/semantic-ui-react/dist/commonjs/elements/Button/Button.d.ts'. ========
Module resolution kind is not specified, using 'NodeJs'.
Loading module 'react' from 'node_modules' folder, target file type 'TypeScript'.
Directory '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/.pnpm/semantic-ui-react@2.1.3_sfoxds7t5ydpegc3knd667wn6m/node_modules/semantic-ui-react/dist/commonjs/elements/Button/node_modules' does not exist, skipping all lookups in it.
Directory '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/.pnpm/semantic-ui-react@2.1.3_sfoxds7t5ydpegc3knd667wn6m/node_modules/semantic-ui-react/dist/commonjs/elements/node_modules' does not exist, skipping all lookups in it.
Resolution for module 'react' was found in cache from location '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/.pnpm/semantic-ui-react@2.1.3_sfoxds7t5ydpegc3knd667wn6m/node_modules/semantic-ui-react/dist/commonjs'.
======== Module name 'react' was successfully resolved to '/Users/dudkevich/job/projects/DudaGod/pnpm-react-issue/node_modules/.pnpm/@types+react@17.0.52/node_modules/@types/react/index.d.ts' with Package ID '@types/react/index.d.ts@17.0.52'. ========
// ...
```

Here react resolves to `@types/react/index.d.ts@17.0.52` and everything works fine.
