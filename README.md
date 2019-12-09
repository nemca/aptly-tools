# aptly-tools

Tools for managing debian repos by [aptly](https://www.aptly.info).

## Example usage
Let's add new repo for Elasticsearch packages. In the [documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html) we can see, that they ask us to add repo to sources.list and import PGP key.  
Adding PGP key
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
and create local mirror
```
aptly-create-imported-repo --repo thirdparty-buster --name elasticsearch7.x --url https://artifacts.elastic.co/packages/7.x/apt/ --distribution stable --component main --with-sources false
```
After this, create crontask for automatic update packages. Important: old version packages in our local repo don' remove.
```
53 03 * * * aptly-mirror-update --repo thirdparty-buster --distribution buster
```
Now we can configure apt sources.list for usage our local repos
```
deb http://your.aptly.host/thirdparty-buster/ buster main
```

Also let's create local copy of Debian repos.
```
aptly-create-imported-repo --repo debian --name main --url http://ftp.ru.debian.org/debian/ --distribution buster --component all
aptly-create-imported-repo --repo debian --name security --url http://security.debian.org/debian-security --distribution buster/updates --component all
aptly-create-imported-repo --repo debian --name updates --url http://ftp.ru.debian.org/debian/ --distribution buster-updates --component all
aptly-create-imported-repo --repo debian --name backports --url http://ftp.ru.debian.org/debian/ --distribution buster-backports --component all
```
and add its update to cron
```
03 03 * * * aptly-mirror-update --repo debian --distribution buster
```
configure sources.list
```
deb http://your.aptly.host/debian/ buster main security updates backports
```

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
  aptly-mirror-update --repo thirdparty-buster --distribution buster
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
