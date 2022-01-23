install-docker
Scripts for docker-machine to install a particular docker version

Add new docker release
Example adding 20.10.7 with diff from 20.10.6:

Run make add-new-version with the previous and new version:
PREVIOUS_ADD_DOCKER_VERSION=20.10.6 ADD_DOCKER_VERSION=20.10.7 make add-new-version
Generate distributed script by running make generate
Under dist/ create/update the proper docker install script symlink <DOCKER_MAJOR>.<DOCKER_MINOR>.sh, to the generated script. Ex: ln -s 20.10.7.sh 20.10.sh
Optional: Run OS tests locally using make test (currently takes around 45 minutes)
Commit changes and submit PR (this will start the tests as well)
Test releases
The repo contains some tests to check if the docker install scripts are working fine on defined distros and versions. The tests are executed within a dind env for every pkg/<DOCKER_VERSION> folder, using the generated scripts to install and run docker on defined distros and versions.

make test

There is the default distros and versions definition to test:

TEST_OS_IMAGE_NAME=(ubuntu centos debian)
TEST_OS_IMAGE_TAG[0]="16.04 18.04 20.04"
TEST_OS_IMAGE_TAG[1]="centos7 centos8 rocky 8"
TEST_OS_IMAGE_TAG[2]="10"
The test definition can be overwritten on every docker version folder, using the shell script file pkg/<DOCKER_VERSION>/config.sh

#!/bin/sh

DOCKER_GIT_COMMIT="3d8fe77c2c46c5b7571f94b42793905e5b3e42e4"

TEST_OS_IMAGE_NAME=(ubuntu centos debian)
TEST_OS_IMAGE_TAG[0]="20.04"
TEST_OS_IMAGE_TAG[1]="centos7"
TEST_OS_IMAGE_TAG[2]="10"
Tip As dind test env doesn't use systemd, dockerd is started manually. The default timeout waiting until dockerd starts, is defined by env variable DIND_TEST_WAIT=3s. It can be overwritten on execution time if required, DIND_TEST_WAIT=5s make test
