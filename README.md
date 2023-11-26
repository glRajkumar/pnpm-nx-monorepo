# Workspace using pnpm

### steps in building this workspace

1) Init node project using pnpm
```sh
pnpm init
```

2) remove "main": "index.js" in `package.json` (optional)

3) create pnpm-workspace.yaml|yml (used common structure used in industry) 
```yml
packages:
  - "apps/*"
  - "packages/*"
```

4) added `apps/demo1` using `pnpm create next-app demo1`.
You can run app using the following options,
 - `pnpm dev` (within that directory)
 - `pnpm --filter|-F {package-name} {command}` (goto root directory). `pnpm -F demo1 dev`
 
5) added `packakes/ui` using `pnpm init`. Then setuped typescript+react simple project by following steps,

- `pnpm add --filter ui react`
- `pnpm add --filter ui typescript @types/node @types/react @types/react-dom -D`
- create `tsconfig.json` file. (if you want you can generate using `tsc --init` command, else you can use the following sample)

```json
{
  "compilerOptions": {
    "jsx": "react-jsx",
    "allowJs": true,
    "esModuleInterop": true,
    "declaration": true, // to export types
    "allowSyntheticDefaultImports": true,
    "module": "commonjs",
    "outDir": "./dist"
  },
  "include": [
    "./src"
  ],
  "exclude": [
    "dist",
    "node_modules",
    "**/*.spec.ts"
  ]
}
```
- create `button`, `index` files in `src/` folder.
- update `package.json` as follows,
  1) change in `main` key, update value `index` -> `dist/index`
  2) add build srcipt. "build": "rm -rf dist && tsc"

6) Now you can build the project. `pnpm -F ui build`, or
  - `pnpm run -r build` (recursive)
  - `pnpm run --parallel -r build`

7) add `ui` refrence to `demo1` using `pnpm add ui -F demo1 --workspace` (it will create `ui` depedency in `demo1/package.json`). (also if you dont want the version of `ui` and then change, `^` to `*` in that worksace value)

8) now you can import `ui` in `demo1`.