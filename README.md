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

## aptly-create-imported-repo
This script create repo and mirror. Update mirror and import packages to repo.
```
aptly-create-imported-repo --help
Usage: aptly-create-imported-repo [options]

Options:
  -a --architectures    list of architectures for publishing
  -c --component        component name or 'all' for all available components (default: main)
  -d --distribution     distribution name (default: buster)
  -h --help             show thin message
  -n --name             name of thirdparty distribution
  -p --publish          publish snapshot (default: false)
  -r --repo             repository name (default: thirdparty-buster)
  -s --with-sources     enable/disable source packages (default: true)
  -u --url              repository url

Example:
  aptly-create-imported-repo --repo thirdparty-buster  --name aptly --url http://repo.aptly.info/ --distribution squeeze --component main
  aptly-create-imported-repo --repo thirdparty-stretch --name rabbitmq --url https://dl.bintray.com/rabbitmq/debian/ --distribution stretch --component main --with-source false
  aptly-create-imported-repo --repo thirdparty-buster --name puppetlabs --url http://apt.puppetlabs.com/ --distribution buster --component "puppet puppet6 puppet-tools" --with-source false
```
