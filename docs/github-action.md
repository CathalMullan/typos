# GitHub Action

If you want an easy way to test your repository spelling (or a subset of files)
you can use the Typos Action! It is served from this repository, and can
easily be used as follows:

```yaml
name: Test GitHub Action
on: [pull_request]

jobs:
  run:
    name: Spell Check with Typos
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Actions Repository
      uses: actions/checkout@v4

    - name: Check spelling of file.txt
      uses: crate-ci/typos@master
      with:
        files: ./file.txt

    - name: Use custom config file
      uses: crate-ci/typos@master
      with:
        files: ./file.txt
        config: ./myconfig.toml

    - name: Ignore implicit configuration file
      uses: crate-ci/typos@master
      with:
        files: ./file.txt
        isolated: true

    - name: Writes changes in the local checkout
      uses: crate-ci/typos@master
      with:
        write_changes: true
```

**Important** for any of the examples above, make sure that you choose
a release or commit as a version, and not a branch (which is a moving target).

## Input

| Name               | Description                                                     | Required | Default                                              |
| ------------------ | --------------------------------------------------------------- | -------- | ---------------------------------------------------- |
| files              | Files or patterns to check                                      | false    | If not defined, the default set of files are checked |
| extend_identifiers | Comma separated list of extend identifiers, like someone's name | false    | not set                                              |
| extend_words       | Comma separated list of extend words.                           | false    | not set                                              |
| isolated           | Ignore implicit configuration files                             | false    | false                                                |
| write_changes      | Writes changes on the Action's local checkout                   | false    | false                                                |
| config             | Use a custom config file (must exist)                           | false    | not set                                              |

`write_changes`: doesn't commit or push anything to the branch. It only writes the changes locally
to disk, and this can be combined with other actions, for instance that will [submit code
suggestions based on that local diff](https://github.com/getsentry/action-git-diff-suggestions).
