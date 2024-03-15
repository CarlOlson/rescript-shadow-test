Here you can see the file layout.  Both `src-helpers` and `test-helpers` contain a `Config` module inside their test directory.  These two configs should not be exported.  `src-helper` opens `test-helpers` and uses its own `Config`.  This all should work.

```
 .
├──  app
│   ├──  src
│   │   └──  Main.res
│   └──  test
│       ├──  Config.res
│       └──  MainTest.res
│
├──  src-helpers
│   ├──  src
│   │   └──  SrcHelper.res
│   └──  test
│       ├──  Config.res
│       └──  MainTest.res
│
└──  test-helpers
    ├──  bsconfig.json
    ├──  package.json
    ├──  src
    │   └──  Helper.res
    └──  test
        └──  Config.res
```

But this causes an error when building `app` with both workspaces pinned:

```
$ yarn workspace app run -T rescript build
yarn run v1.22.17
$ <removed>/rescript-shadow-test/node_modules/.bin/rescript build
>>>> Start compiling
Warning: bsconfig.json is deprecated. Migrate it to rescript.json

Dependency pinned on test-helpers
Dependency pinned on src-helpers
rescript: [1/1] test/MainTest-SrcHelpers.cmj
FAILED: test/MainTest-SrcHelpers.cmj

  We've found a bug for you!
  <removed>/rescript-shadow-test/src-helpers/test/MainTest.res:4:14-23

  2 │
  3 │ let _ = Helper.setup()
  4 │ let config = Config.get()
  5 │

  The module or file Config can't be found.
  - If it's a third-party dependency:
    - Did you add it to the "bs-dependencies" or "bs-dev-dependencies" in bsconfig.json?
  - Did you include the file's directory to the "sources" in bsconfig.json?


  Hint: Did you mean Config or Config?

FAILED: cannot make progress due to previous errors.
Failure: <removed>/rescript-shadow-test/node_modules/rescript/linux/ninja.exe
Location: <removed>/rescript-shadow-test/node_modules/src-helpers/lib/bs
>>>> Finish compiling (exit: 1)
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```
