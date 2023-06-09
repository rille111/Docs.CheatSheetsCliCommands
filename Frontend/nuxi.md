# Nuxi

A Nuxt CLI (nicknamed Nuxi) was introduced to provide a no dependency experience for easily scaffolding your Nuxt projects. This article seeks to explore Nuxi and introduce you to some of its features.

* Getting started (https://vueschool.io/articles/vuejs-tutorials/getting-started-with-nuxi-nuxt-cli/)
* Installation(
    * `pnpm install nuxi -g` (installs globally)
    * Or: `pnpm dlx nuxi` (downloads script and executes it)
    * Refresh Env or Reboot
	
* `./apps/pnpm dlx nuxi init WEBSITE1` - pnpm dlx. Fetches a package from the registry without installing it as a dependency, hotloads it, and runs whatever default command binary it exposes.
* `npx nuxi clean` - cleans cache