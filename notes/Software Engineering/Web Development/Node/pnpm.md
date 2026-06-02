# pnpm

## Dependencies
[silly bugs](https://www.kochan.io/nodejs/pnpms-strictness-helps-to-avoid-silly-bugs.html)
[symlinks](https://pnpm.io/symlinked-node-modules-structure)


This is a package manager similar to `npm` and `yarn` for Node.js projects.

It solves the issues of `npm`'s `flattened dependency tree` where all of the transient dependencies are listed on the root level of your `node_modules` directory.

e.g. installing express leads to all this other stuff:

```
accepts
array-flatten
content-disposition
content-type
cookie
cookie-signature
debug
depd
destroy
ee-first
encodeurl
escape-html
etag
express
finalhandler
forwarded
fresh
http-errors
inherits
ipaddr.js
media-typer
merge-descriptors
methods
mime
mime-db
mime-types
ms
negotiator
on-finished
parseurl
path-to-regexp
proxy-addr
qs
range-parser
send
serve-static
setprototypeof
statuses
type-is
unpipe
utils-merge
vary
```

You can reference any of these other packages in your project. But if express removes one of them in a newer version, then you'll have a mysterious error.

pnpm handles package management differently to npm and yarn. It has a list of first level dependencies and then in .pnpm it has all the transitive dependencies.

It then uses symlinks to link it all together.

```
node_modules
├── foo -> ./.pnpm/foo@1.0.0/node_modules/foo
└── .pnpm
    ├── bar@1.0.0
    │   └── node_modules
    │       └── bar -> <store>
    └── foo@1.0.0
        └── node_modules
            ├── foo -> <store>
            └── bar -> ../../bar@1.0.0/node_modules/bar // this symlink is ignored by Node. It resolves by finding it under .pnpm/bar
```

## Implementations

## Dependents


## Interactions