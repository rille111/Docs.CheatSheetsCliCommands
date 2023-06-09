# Docker & Containerization

* https://nodejs.org/en/docs/guides/nodejs-docker-webapp
* https://github.com/g-plane/pnpm-docker#supported-tags
* https://dev.to/nklsw/how-to-create-a-dockerized-nuxt-3-development-environment-1p0a
* https://dockerize.io/guides/docker-nuxtjs-guide
* https://towardsserverless.com/articles/deploying-nuxt-3-ssr-webapp-with-docker

## Dockerfile example

``` Docker
FROM gplane/pnpm:8.6.0-node18-alpine as build
# update and install the latest dependencies
RUN apk update && apk upgrade

# set work dir as app
WORKDIR /app
# copy the nuxt project content
COPY . /app
COPY . ./

# must do this otherwise the build will fail due to .. some reason
RUN rm -rf .output && rm -rf .nuxt 
# install all the project npm dependencies
RUN pnpm i
# build the nuxt project to generate the artifacts in .output directory
RUN pnpm build

# we are using multi stage build process to keep the image size as small as possible
FROM node:18-alpine3.17
# update and install latest dependencies, add dumb-init package
RUN apk update && apk upgrade && apk add dumb-init && adduser -D nuxtuser 

# set work dir as app
WORKDIR /app
# copy the output directory to the /app directory from 
COPY --from=build /app/.output ./

# expose 8080 on container
EXPOSE 8080
# set app host and port . In nuxt 3 this is based on nitor and you can read more on this https://nitro.unjs.io/deploy/node#environment-variables
ENV HOST=0.0.0.0 PORT=8080 NODE_ENV=production
# start the app with dumb init to spawn the Node.js runtime process, with signal support
CMD ["dumb-init","node","/app/server/index.mjs"]
```

## Building, Pushing, Running

* Build:
	* ```docker build -t ws-containers/website-retailers:latest .```
* Push:
	* ```docker tag  ws-containers/website-retailers:latest      europe-north1-docker.pkg.dev/wheelsearcher/ws-containers/website-retailers:latest```
	* ```docker push europe-north1-docker.pkg.dev/wheelsearcher/ws-containers/website-retailers:latest```
* Run:
	* ```docker run -p 3050:8080 -it --rm ws-containers/website-retailers:latest```
