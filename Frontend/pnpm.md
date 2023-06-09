# pnpm

Fast, disk space efficient package manager.
* [Installation](https://pnpm.io/installation)
    * Windows: `choco install pnpm`§
	* `pnpm setup`
    * Refresh Env or Reboot
* `pnpm install --shamefully-hoist` This flag: All the subdeps will be hoisted into the root node_modules. Your code will have access to them

### pnpm - setup

Only needs to be done once

* `pnpm init` - creates package.json
* Create `apps` and `packages` folder
* Create a `pnpm-workspace.yaml` file to config the workspace, defining the monorepo structure
* Create `.npmrc` with `shamefully-hoist=true` in it
* Ignore `node_modules` in `.gitignore`

## example commands

* `pnpm i --shamefully-hoist @fortawesome/free-brands-svg-icons`

## Notes

Inside a pnpm Workspace, pnpm install installs all dependencies in all the projects. So this will install dependencies in our website1 and website2 applications.

* `pnpx` = `pnpm dlx`
* `pnpm create` is short for `pnpm dlx`
    * Example: `pnpm dlx create-react-app my-app`


## Workspace via pnpm (unused!!)

### Setup 
Create a website
* `./apps/pnpm dlx nuxi init WEBSITE1` - pnpm dlx. Fetches a package from the registry without installing it as a dependency, hotloads it, and runs whatever default command binary it exposes.

Create a package
* `./packages/somepackage/pnpm init` - initalize
* `./packages/somepackage/pnpm i @nuxt/kit` - to be able to define our nuxt Module
* Change port & name for all applications in `package.json`
* Next, create an index.ts file in the root of the package and make it an entry point in.
    ``` typescript
    // packages/somepackage> index.ts
    import { defineNuxtModule } from '@nuxt/kit';

    export default defineNuxtModule({
        setup(_, nuxt) {
            // here we need to setup our components
        }
    })
    ```
* Next, configure packages.json to use index.ts as an entrypoint
    ``` json
        "main": "index.ts",
    ```
* The best practice is to create the Btn inside the `lib/components` folder.
* Install this module in our applications using:
    * `pnpm i nuxt3-websites-package --filter WEBSITE1`
    * `pnpm i nuxt3-websites-package --filter WEBSITE2`
* Then Register these modules in `nuxt.config.ts` in the application:
    ``` typescript
    export default defineNuxtConfig({
        modules: ['nuxt3-websites-package']
    })
    ```
	
* To run all applications, do:
    * `pnpm run -r dev`, -r refers to “recursively”, so this runs the “dev” command from each package's "scripts" object. If a package doesn't have the command, it is skipped.
