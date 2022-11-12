### FyDevops 
Toolset for  environment of an integrated modeling software suite
#### Purpose
- To provide a base environment for the management and distribution of integrated modeling software suites on different stations for the realization of magnetic confinement fusion research.
#### Features
- From the point of view of reproducibility, the first step is to maintain the consistency of the underlying software environment, scripting is fundamental to improve the reliability of the automation.
- Launching virtualized environments at different sites, supporting syncing of software environments on multi-sites.
#### functionality
- Start a virtual environment with a compatibility layer, e.g.
    - centos
        - 7
        - 8
    - ubutun
- Automatically build the base package management tool (e.g. easybuild):
    - . /scripts/fy_bootstrap.sh
#### Usage
##### build base image

```bash
docker build --build-arg FY_UID=$(id -u) --build-arg FY_GID=$(id -g)  -t fybase:ubuntu.focal .
```

##### mount volume

```bash
docker run --rm -it \
--mount type=bind,source=/gpfs/fuyun_repos/ubuntu/focal/,target=/fuyun \
--mount type=bind,source=/gpfs/fuyun_sources/,target=/fuyun/sources,readonly \
fybase:ubuntu.focal
```

##### build EasyBuild in docker
```bash
sudo groupadd -g 504 develop
sudo usermod -g develop fuyun

bash ./scripts/fy_bootstrap.sh

source ${EASYBUILD_PREFIX}/software/lmod/lmod/init/bash
module use ${EASYBUILD_PREFIX}/modules/all/

export EASYBUILD_PREFIX=/fuyun
export EASYBUILD_ROBOT_PATHS=/fuyun/sources/ebfiles/FyDevOps/easybuild/easyconfigs/:/fuyun/sources/ebfiles/imas_ebs/easybuild/easyconfigs/:/fuyun/sources/ebfiles/easybuild-easyconfigs/:$EBROOTEASYBUILD/easybuild/easyconfigs
export EASYBUILD_BUILDPATH=/tmp/eb_build_${USERID}
```
##### deploy research sofware in docker 
- Refer to the method described in the FyBuild repository(https://github.com/fusion-yun/FyBuild)

##### rsync the result to destination site
```bash
sh ./scripts/scripts/rsync_to_site.sh
```
