## Merging Tenants Issue with yarn

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