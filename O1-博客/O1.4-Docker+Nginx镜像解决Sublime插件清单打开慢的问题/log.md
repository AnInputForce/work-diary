```shell
ChinaDreams:rap2-delos kangcunhua$ docker-compose up -d
Building delos
Step 1/4 : FROM node:9.11.1-alpine
 ---> 35a36bd2d140
Step 2/4 : WORKDIR /app
 ---> Using cache
 ---> 154785a9403b
Step 3/4 : ADD . /tmp
 ---> 51b9d022db53
Step 4/4 : RUN /bin/sh -c 'cd /tmp && npm start && mv ./dist/* /app/ && mv ./node_modules /app/ && rm -rf /tmp'
 ---> Running in cbe38de4d191

> rap2-delos@1.0.0 start /tmp
> cross-env NODE_ENV=production pm2 start dist/dispatch.js --name=rap-server-delos

sh: cross-env: not found
npm ERR! file sh
npm ERR! code ELIFECYCLE
npm ERR! errno ENOENT
npm ERR! syscall spawn
npm ERR! rap2-delos@1.0.0 start: `cross-env NODE_ENV=production pm2 start dist/dispatch.js --name=rap-server-delos`
npm ERR! spawn ENOENT
npm ERR! 
npm ERR! Failed at the rap2-delos@1.0.0 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
npm WARN Local package.json exists, but node_modules missing, did you mean to install?

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2018-06-03T07_44_13_307Z-debug.log
ERROR: Service 'delos' failed to build: The command '/bin/sh -c /bin/sh -c 'cd /tmp && npm start && mv ./dist/* /app/ && mv ./node_modules /app/ && rm -rf /tmp'' returned a non-zero code: 1
ChinaDreams:rap2-delos kangcunhua$ docker-compose stop
ChinaDreams:rap2-delos kangcunhua$ docker-compose rm
No stopped containers
ChinaDreams:rap2-delos kangcunhua$ docker-compose up -d
Creating rap2-mysql ... done
Creating rap2-redis ... done
Creating rap2-delos ... done
ChinaDreams:rap2-delos kangcunhua$ docker-compose stop
Stopping rap2-delos ... done
Stopping rap2-redis ... done
Stopping rap2-mysql ... done
ChinaDreams:rap2-delos kangcunhua$ docker-compose rm
Going to remove rap2-delos, rap2-redis, rap2-mysql
Are you sure? [yN] y
Removing rap2-delos ... done
Removing rap2-redis ... done
Removing rap2-mysql ... done
ChinaDreams:rap2-delos kangcunhua$ docker-compose up -d
Building delos
Step 1/4 : FROM node:9.11.1-alpine
 ---> 35a36bd2d140
Step 2/4 : WORKDIR /app
 ---> Using cache
 ---> 154785a9403b
Step 3/4 : ADD . /tmp
 ---> 98338fca0f3b
Step 4/4 : RUN /bin/sh -c 'cd /tmp && npm start && mv ./dist/* /app/ && mv ./node_modules /app/ && rm -rf /tmp'
 ---> Running in d2e77b4876bd

> rap2-delos@1.0.0 start /tmp
> NODE_ENV=production pm2 start dist/dispatch.js --name=rap-server-delos

sh: pm2: not found
npm ERR! file sh
npm ERR! code ELIFECYCLE
npm ERR! errno ENOENT
npm ERR! syscall spawn
npm ERR! rap2-delos@1.0.0 start: `NODE_ENV=production pm2 start dist/dispatch.js --name=rap-server-delos`
npm ERR! spawn ENOENT
npm ERR! 
npm ERR! Failed at the rap2-delos@1.0.0 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
npm WARN Local package.json exists, but node_modules missing, did you mean to install?

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2018-06-03T08_11_24_507Z-debug.log
ERROR: Service 'delos' failed to build: The command '/bin/sh -c /bin/sh -c 'cd /tmp && npm start && mv ./dist/* /app/ && mv ./node_modules /app/ && rm -rf /tmp'' returned a non-zero code: 1
ChinaDreams:rap2-delos kangcunhua$ npm 
```





```shell
ChinaDreams:rap2-delos kangcunhua$ docker-compose up -d
Building delos
Step 1/4 : FROM node:9.11.1-alpine
 ---> 35a36bd2d140
Step 2/4 : WORKDIR /app
 ---> Using cache
 ---> 154785a9403b
Step 3/4 : ADD . /tmp
 ---> 98338fca0f3b
Step 4/4 : RUN /bin/sh -c 'cd /tmp && npm start && mv ./dist/* /app/ && mv ./node_modules /app/ && rm -rf /tmp'
 ---> Running in d2e77b4876bd

> rap2-delos@1.0.0 start /tmp
> NODE_ENV=production pm2 start dist/dispatch.js --name=rap-server-delos

sh: pm2: not found
npm ERR! file sh
npm ERR! code ELIFECYCLE
npm ERR! errno ENOENT
npm ERR! syscall spawn
npm ERR! rap2-delos@1.0.0 start: `NODE_ENV=production pm2 start dist/dispatch.js --name=rap-server-delos`
npm ERR! spawn ENOENT
npm ERR! 
npm ERR! Failed at the rap2-delos@1.0.0 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
npm WARN Local package.json exists, but node_modules missing, did you mean to install?

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2018-06-03T08_11_24_507Z-debug.log
ERROR: Service 'delos' failed to build: The command '/bin/sh -c /bin/sh -c 'cd /tmp && npm start && mv ./dist/* /app/ && mv ./node_modules /app/ && rm -rf /tmp'' returned a non-zero code: 1
ChinaDreams:rap2-delos kangcunhua$ npm install
npm WARN deprecated socks@1.1.10: If using 2.x branch, please upgrade to at least 2.1.6 to avoid a serious bug with socket data flow and an import issue introduced in 2.1.0
WARN registry Unexpected warning for https://registry.npm.taobao.org/: Miscellaneous Warning EINTEGRITY: sha1-QFUCsAfzGcP0cXXER0UnMA8qta0= integrity checksum failed when using sha1: wanted sha1-QFUCsAfzGcP0cXXER0UnMA8qta0= but got sha512-zr6QQnzLt3Ja0t0XI8gws2kn7zV2p0l/D3kreNvS6hFZhVU5g+uY/30l42jbgt0XGcNBEmBDGJR71J692V92tA==. (260 bytes)
WARN registry Using stale package data from https://registry.npm.taobao.org/ due to a request error during revalidation.

> fsevents@1.2.4 install /Users/kangcunhua/Documents/work-space/docker-project/rap2-delos/node_modules/fsevents
> node install

[fsevents] Success: "/Users/kangcunhua/Documents/work-space/docker-project/rap2-delos/node_modules/fsevents/lib/binding/Release/node-v59-darwin-x64/fse.node" is installed via remote

> hiredis@0.5.0 install /Users/kangcunhua/Documents/work-space/docker-project/rap2-delos/node_modules/hiredis
> node-gyp rebuild

xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance

xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance

  CC(target) Release/obj.target/hiredis-c/deps/hiredis/sds.o
  CC(target) Release/obj.target/hiredis-c/deps/hiredis/read.o
  LIBTOOL-STATIC Release/hiredis-c.a
  CXX(target) Release/obj.target/hiredis/src/hiredis.o
  CXX(target) Release/obj.target/hiredis/src/reader.o
  SOLINK_MODULE(target) Release/hiredis.node

> pre-commit@1.2.2 install /Users/kangcunhua/Documents/work-space/docker-project/rap2-delos/node_modules/pre-commit
> node install.js


> spawn-sync@1.0.15 postinstall /Users/kangcunhua/Documents/work-space/docker-project/rap2-delos/node_modules/spawn-sync
> node postinstall


> nodemon@1.17.5 postinstall /Users/kangcunhua/Documents/work-space/docker-project/rap2-delos/node_modules/nodemon
> node bin/postinstall || exit 0

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN The package @types/kcors is included as both a dev and production dependency.
npm WARN The package @types/koa is included as both a dev and production dependency.
npm WARN The package @types/koa-router is included as both a dev and production dependency.
npm WARN The package @types/mocha is included as both a dev and production dependency.

added 1198 packages in 126.486s
ChinaDreams:rap2-delos kangcunhua$ docker-compose up -d
Building delos
Step 1/4 : FROM node:9.11.1-alpine
 ---> 35a36bd2d140
Step 2/4 : WORKDIR /app
 ---> Using cache
 ---> 154785a9403b
Step 3/4 : ADD . /tmp
 ---> a4782410d241
Step 4/4 : RUN /bin/sh -c 'cd /tmp && npm start && mv ./dist/* /app/ && mv ./node_modules /app/ && rm -rf /tmp'
 ---> Running in a9da161359ae

> rap2-delos@1.0.0 start /tmp
> NODE_ENV=production pm2 start dist/dispatch.js --name=rap-server-delos


                        -------------

__/\\\\\\\\\\\\\____/\\\\____________/\\\\____/\\\\\\\\\_____
 _\/\\\/////////\\\_\/\\\\\\________/\\\\\\__/\\\///////\\\___
  _\/\\\_______\/\\\_\/\\\//\\\____/\\\//\\\_\///______\//\\\__
   _\/\\\\\\\\\\\\\/__\/\\\\///\\\/\\\/_\/\\\___________/\\\/___
    _\/\\\/////////____\/\\\__\///\\\/___\/\\\________/\\\//_____
     _\/\\\_____________\/\\\____\///_____\/\\\_____/\\\//________
      _\/\\\_____________\/\\\_____________\/\\\___/\\\/___________
       _\/\\\_____________\/\\\_____________\/\\\__/\\\\\\\\\\\\\\\_
        _\///______________\///______________\///__\///////////////__


                          Community Edition

            Production Process Manager for Node.js applications
                     with a built-in Load Balancer.


                Start and Daemonize any application:
                $ pm2 start app.js

                Load Balance 4 instances of api.js:
                $ pm2 start api.js -i 4

                Monitor in production:
                $ pm2 monitor

                Make pm2 auto-boot at server restart:
                $ pm2 startup

                To go further checkout:
                http://pm2.io/


                        -------------

[PM2] Spawning PM2 daemon with pm2_home=/root/.pm2
[PM2] PM2 Successfully daemonized
[PM2][ERROR] script not found : /tmp/dist/dispatch.js
script not found : /tmp/dist/dispatch.js
┌──────────┬────┬──────┬─────┬────────┬─────────┬────────┬─────┬─────┬──────┬──────────┐
│ App name │ id │ mode │ pid │ status │ restart │ uptime │ cpu │ mem │ user │ watching │
└──────────┴────┴──────┴─────┴────────┴─────────┴────────┴─────┴─────┴──────┴──────────┘
 Use `pm2 show <id|name>` to get more details about an app
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! rap2-delos@1.0.0 start: `NODE_ENV=production pm2 start dist/dispatch.js --name=rap-server-delos`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the rap2-delos@1.0.0 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2018-06-03T08_17_08_550Z-debug.log
ERROR: Service 'delos' failed to build: The command '/bin/sh -c /bin/sh -c 'cd /tmp && npm start && mv ./dist/* /app/ && mv ./node_modules /app/ && rm -rf /tmp'' returned a non-zero code: 1
ChinaDreams:rap2-delos kangcunhua$ 
```







```shell
ChinaDreams:rap2-delos kangcunhua$ npm run build

> rap2-delos@1.0.0 build /Users/kangcunhua/Documents/work-space/docker-project/rap2-delos
> rimraf -rf dist/ && tsc

node_modules/sequelize-typescript/lib/utils/types.d.ts:9:16 - error TS2344: Type 'keyof T' does not satisfy the constraint 'string'.
  Type 'string | number | symbol' is not assignable to type 'string'.
    Type 'number' is not assignable to type 'string'.

9     [P in Diff<keyof T, K>]: T[P];
                 ~~~~~~~


src/routes/organization.ts:160:9 - error TS2339: Property 'prevAssociations' does not exist on type 'IRouterContext'.

160     ctx.prevAssociations = await reloaded.$get('members')
            ~~~~~~~~~~~~~~~~


src/routes/organization.ts:162:9 - error TS2339: Property 'nextAssociations' does not exist on type 'IRouterContext'.

162     ctx.nextAssociations = await reloaded.$get('members')
            ~~~~~~~~~~~~~~~~


src/routes/organization.ts:177:12 - error TS2339: Property 'prevAssociations' does not exist on type 'IRouterContext'.

177   if (!ctx.prevAssociations || !ctx.nextAssociations) return
               ~~~~~~~~~~~~~~~~


src/routes/organization.ts:177:37 - error TS2339: Property 'nextAssociations' does not exist on type 'IRouterContext'.

177   if (!ctx.prevAssociations || !ctx.nextAssociations) return
                                        ~~~~~~~~~~~~~~~~


src/routes/organization.ts:178:21 - error TS2339: Property 'prevAssociations' does not exist on type 'IRouterContext'.

178   let prevIds = ctx.prevAssociations.map((item: any) => item.id)
                        ~~~~~~~~~~~~~~~~


src/routes/organization.ts:179:21 - error TS2339: Property 'nextAssociations' does not exist on type 'IRouterContext'.

179   let nextIds = ctx.nextAssociations.map((item: any) => item.id)
                        ~~~~~~~~~~~~~~~~


src/routes/repository.ts:256:9 - error TS2339: Property 'prevAssociations' does not exist on type 'IRouterContext'.

256     ctx.prevAssociations = reloaded.members
            ~~~~~~~~~~~~~~~~


src/routes/repository.ts:259:9 - error TS2339: Property 'nextAssociations' does not exist on type 'IRouterContext'.

259     ctx.nextAssociations = reloaded.members
            ~~~~~~~~~~~~~~~~


src/routes/repository.ts:285:12 - error TS2339: Property 'prevAssociations' does not exist on type 'IRouterContext'.

285   if (!ctx.prevAssociations || !ctx.nextAssociations) return
               ~~~~~~~~~~~~~~~~


src/routes/repository.ts:285:37 - error TS2339: Property 'nextAssociations' does not exist on type 'IRouterContext'.

285   if (!ctx.prevAssociations || !ctx.nextAssociations) return
                                        ~~~~~~~~~~~~~~~~


src/routes/repository.ts:286:21 - error TS2339: Property 'prevAssociations' does not exist on type 'IRouterContext'.

286   let prevIds = ctx.prevAssociations.map((item: any) => item.id)
                        ~~~~~~~~~~~~~~~~


src/routes/repository.ts:287:21 - error TS2339: Property 'nextAssociations' does not exist on type 'IRouterContext'.

287   let nextIds = ctx.nextAssociations.map((item: any) => item.id)
                        ~~~~~~~~~~~~~~~~


npm ERR! code ELIFECYCLE
npm ERR! errno 2
npm ERR! rap2-delos@1.0.0 build: `rimraf -rf dist/ && tsc`
npm ERR! Exit status 2
npm ERR! 
npm ERR! Failed at the rap2-delos@1.0.0 build script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/kangcunhua/.npm/_logs/2018-06-03T08_41_56_974Z-debug.log
```

