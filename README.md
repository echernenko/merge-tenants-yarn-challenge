## Merging Tenants Challenge with yarn

TLDR;

Having one tenant:

```
{
  "name": "tenant-1",
  "dependencies": {
    "rimraf": "^3.0.2"
  },
  "resolutions": {
    "//comment": "rimraf depends on minimatch",
    "minimatch": "5.0.0",
    "//comment": "minimatch depends on brace-expansion",
    "brace-expansion": "2.0.0"
  }
}
```

and another:

```
{
  "name": "tenant-2",
  "dependencies": {
    "rimraf": "2.7.0"
  },
  "resolutions": {
    "//comment": "rimraf depends on minimatch",
    "minimatch": "4.0.0",
    "//comment": "minimatch depends on brace-expansion",
    "brace-expansion": "1.1.5"
  }
}
```

conslusions:
- rimraf dependency versions are different (^3.0.2 vs 2.7.0)
- dependency of dependency versions via resolutions key are very different: minimatch (5.0.0 vs 4.0.0)
- dependency of dependency of dependency versions are very different: brace-expansion (2.0.0 vs 1.1.5)
- merging tenants become very complicated without deep refactoring and re-working business logic


raw notes on reproducing challenge:

```
mkdir merging-tenants-issue
cd merging-tenants-issue
mkdir tenant-1
mkdir tenant-2
cd tenant-1
yarn init
yarn add rimraf
cd ../tenant-2
yarn init
yarn add rimraf

# selective dependency resolution
# https://classic.yarnpkg.com/lang/en/docs/selective-version-resolutions/

# add to tenant-1 package.json
"resolutions": {
   "balanced-match": "1.0.0"
}

# add to tenant-2 package.json
"resolutions": {
   "balanced-match": "2.0.0"
}

# run in both tenants
yarn install

# tenant-1 downgrade from "balanced-match": "1.0.2" to "balanced-match": "1.0.0" goes fine
# tenant-2 complains (just a warning)
# warning Resolution field "balanced-match@2.0.0" is incompatible with requested version "balanced-match@^1.0.0"

# now we'd like to merge tenant-1 and tenant-2
# we probably would like to get rid of "resolutions" key in manifest package.json at all
# it is impossible due to compliance reasons
# we could adopt "resolutions": { "balanced-match": "2.0.0" }
# but this will force rewriting business logic of the tenants
#
# how to repro recursion - facing challenges resolutions of resolutions?
# can you help?

```
