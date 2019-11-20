# aptly-tools

Tools for managing debian repos by [aptly](https://www.aptly.info).

## aptly-mirror-update
This script do update local mirrors and import new packages to local repository, then create snapshot and publish it.
```
aptly-mirror-update --help
Usage: aptly-mirror-update [options]

Options:
  -d --distribution    distribution name (default: buster)
  -f --force-yes       do not ask whether to publish, assume answer 'yes'
  -h --help            show thin message
  -r --repo            repository name (default: avito)
  -q --quiet           do not write anything to standard output

Example:
  aptly-mirror-update --repo debian --distribution buster
```
