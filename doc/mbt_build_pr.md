## mbt build pr

Build the modules changed in dst branch relatively to src branch

### Synopsis


Build the modules changed in dst branch relatively to src branch

This command works out the merge base for src and dst branches and
builds all modules impacted by the diff between merge base and
the tip of dst branch.

In addition to the modules impacted by changes, this command also
builds their dependents.

	

```
mbt build pr --src <branch> --dst <branch> [flags]
```

### Options

```
      --dst string   Destination branch
  -h, --help         help for pr
      --src string   Source branch
```

### Options inherited from parent commands

```
      --debug       Enable debugging
      --in string   Path to repo
```

### SEE ALSO
* [mbt build](mbt_build.md)	 - Main command for building the repository

###### Auto generated by spf13/cobra on 20-Apr-2018
