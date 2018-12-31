# meta-sync
Plugin for meta to sync parent directories and the child projects. This
is a development paradigm whereby sub-projects share files and directories
which should be synced amongst them when committing or pushing.

Usage:

```
meta sync
```

## Hooks

meta-sync comes with a set of git hooks which will ensure the following:
* It will complain if you use `git commit`, `git push`, or `git checkout`
instead of `meta git commit`, `meta git push` or `meta git checkout`.
* It will automatically call `meta sync` whenever you are about to push
your commits. If the sync caused any file changes, it will abort the push.
(You can just push again if you want to ignore it since the files will
already be synced.)

You can create these hooks by calling `meta sync copyhook`. If you
already have hooks, you will be given the option to override the existing
ones, merge them (by inserting the hook contents after the hashbang)
or skip.

## Configuration

The directories to sync need to live in the `.meta` file in the parent
repo. The format is as follows:
```json
{
  "config": {
    "sync": {
      "some-path/some-file": "*",
      "some-path/some-directory": ["project1", "project2"],
      "some-path/*.rb": ["project1", "project3"]
    }
  }
}
```

A directory path implies all files recursively in that directory, i.e. it is 
equivalent to `some-path/some-directory/**/*`. A "*" in the value indicates
the file or directory should be shared across all subprojects.

If no configuration is provided, the following configuration is inferred:
```json
{
  "config": {
    "sync": {
      "shared": "*"  
    }
  }
}
```

## Note

`meta-sync` will use the current project as the source of the files to sync
over. Deleting a file will sync the deletion to the other projects.
