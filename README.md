# npm-linked-install-repro

When using `install-strategy=linked` the package namespace is being dropped from workspace packages which causes imports to fail

I have two packages in a workspace in this repo:

- `@iansu/package-a`
- `@iansu/package-b`

`@iansu/package-b` has a dependency on `@iansu/package-a`

When no install strategy is specified this is what the root `node_modules` directory looks like:

```txt
❯ tree node_modules
node_modules
└── @iansu
    ├── package-a -> ../../packages/package-a
    └── package-b -> ../../packages/package-b

3 directories, 0 files
```

In this case nothing is installed in `package-b`'s `node_modules` directory

When `install-strategy=linked` is specific this is what the root `node_modules` directory looks like:

```txt
❯ tree node_modules
node_modules
├── package-a -> ../packages/package-a
└── package-b -> ../packages/package-b

2 directories, 0 files
```

Additionally this is what `package-b`'s `node_modules directory looks like:

```txt
❯ tree packages/package-b/node_modules
packages/package-b/node_modules
└── package-a -> ../../package-a

1 directory, 0 files
```

When I try to import from `@iansu/package-a` I get an error because neither `node_modules/@iansu/package-a` or `packages/package-b/node_modules/@iansu/package-a` exist