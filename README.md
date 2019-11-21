# Jenkins backup script

[![Build Status](https://travis-ci.org/sue445/jenkins-backup-script.svg?branch=master)](https://travis-ci.org/sue445/jenkins-backup-script)

Archive Jenkins settings and plugins

* `$JENKINS_HOME/*.xml`
* `$JENKINS_HOME/jobs/*/*.xml`
* `$JENKINS_HOME/nodes/*`
* `$JENKINS_HOME/plugins/*.jpi`
* `$JENKINS_HOME/secrets/*`
* `$JENKINS_HOME/users/*`

# Usage
```sh
./jenkins-backup.sh /path/to/jenkins_home archive.tar.gz

# add timestamp suffix
./jenkins-backup.sh /path/to/jenkins_home /path/to/destination
```

# run with Jenkins Job
## 1. install Exclusive Execution Plugin
https://wiki.jenkins-ci.org/display/JENKINS/Exclusive+Execution+Plugin

## 2. New Job
![img](http://cdn-ak.f.st-hatena.com/images/fotolife/s/sue445/20131208/20131208001948.png)

## 3. Configure
### Source Code Management > Repository URL
```
https://github.com/sue445/jenkins-backup-script.git
```

* **Recommended** : specify Branch Specifier with latest release tag
* Check https://github.com/sue445/jenkins-backup-script/releases

![0.0.3](http://f.st-hatena.com/images/fotolife/s/sue445/20140331/20140331010645.png)

### Build Triggers > Build periodically
![img](http://cdn-ak.f.st-hatena.com/images/fotolife/s/sue445/20131110/20131110180825.png)

### Build Environment > Set exclusive Execution
![img](http://cdn-ak.f.st-hatena.com/images/fotolife/s/sue445/20131110/20131110194540.png)

### Build > Execute shell
![img](http://cdn-ak.f.st-hatena.com/images/fotolife/s/sue445/20131110/20131110193935.png)

ex.

```bash
./jenkins-backup.sh $JENKINS_HOME /path/to/destination/
```

# Operability confirmed
* Debian jessie
* Debian stretch
* CentOS 7

## Testing
requirements [Docker](https://www.docker.com/)

```sh
bundle install

bundle exec itamae docker --node-yaml=spec/node.yml spec/recipes/bootstrap.rb --image=centos:7 --tag local:latest
DOCKER_IMAGE=local:latest bundle exec rspec
```

# Tips
## rotate backup files
```bash
# keep backup with latest 30 days
find /path/to/backup_* -mtime +30 -delete
```

## Restore commands
example

```bash
sudo /etc/init.d/jenkins stop
cd /path/to/backup_dir
sudo tar xzvf /path/to/destination/tar_file  -C /
sudo /etc/init.d/jenkins start
```

# Changelog
[CHANGELOG.md](CHANGELOG.md)
