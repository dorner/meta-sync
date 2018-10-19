# meta-sync
Plugin for meta to sync parent directories and the child projects. This
is a development paradigm whereby the parent project has a shared directory
which all sub-projects need to have and should always be kept up to date.

By default, `meta-sync` will look in a directory called `shared` and
will copy the contents of that directory to all sub-projects. E.g.
if there are the following files:
* `parent/shared/myfile`
* `parent/shared/another/file`

`meta-sync` will copy these to 
* `parent/sub-project/myfile`
* `parent/sub-project/another/file`

Usage:

```
meta sync
```

## Hooks

meta-sync comes with a set of git hooks which will ensure the following:
* It will complain if you use `git commit`, `git push`, or `git checkout`
instead of `meta git commit`, `meta git push` or `meta git checkout`.
You can opt to continue your command.
* It will automatically call `meta sync` whenever you are about to push
your commits. If the sync caused any file changes, it will abort the push.
(You can just push again if you want to ignore it since the files will
already be synced.)

You can create these hooks by calling `meta sync copyhook`. If you
already have hooks, you will be given the option to override the existing
ones, merge them (by inserting the hook contents after the hashbang)
or skip.

## Configuration

meta-sync uses [meta-config](https://github.com/dorner/meta-config) to configure
the share settings.

```bash
# Set the directory to share. Default is ./shared
meta config sharedDir ./my-shared-dir
```

## Note

If running `meta sync` from inside a child project, the plugin will
sync the *child's files* to the parent and all siblings. If running
from the inside the parent, it will sync the parent to all children.
