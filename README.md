# docker-centos-sonarr
## Sonarr running on the latest CentOS docker image (7.4)
### Sonarr Build: 2.0.0.5163
### Build Version: 8
Date of Build: 22nd March 2018

The Dockerfile should intialise the CentOS image and subscribe to the EPEL repository. The pre-requisites for Sonarr are then installed via yum.

The EPEL repository provides:

    supervisor mediainfo libzen libmediainfo ffmpeg

The Sonarr daemon is controlled via the supervisord daemon which has a web front end exposed via port 9010. Default username and password for the web front end is admin:admin.

The Sonarr software package is downloaded as a tarball file from the Sonarr website and then extracted into the docker container ready for use.

The container can be run as follows:

    docker pull jervine/docker-centos-sonarr
    docker run -d -n <optional name of container> -h <optional host name of container> -e USER="<user account to run as> -e USERUID="<uid of user account"> -e TZ="<optional timezone> -v /<config directory on host>:/config/ -v /<download directory on host>:/downloads -v /<media directory on host>:/media -p 8989:8989 -p 9010:9010 jervine/docker-centos-sonarr

The USER and USERUID variables will be used to create an unprivileged account in the container to run the Sonarr under. The startup.sh script will create this user and also inject the username into the user= parameter of the sonarr.ini supervisor file.

THe TZ variable allows the user to set the correct timezone for the container and should take the form "Europe/London". If no timezone is specified then UTC is used by default. The timezone is set up when the container is run. Subsequent stops and starts will not change the timezone.

If the container is removed and is set up again using docker run commands, remember to remove the .setup file so that the start.sh script will recreate the user account and set the local time correctly.

The container can be verified on the host by using:

    docker logs <container id/container name>

Please note that the SELinux permissions of the config and downloads directories may need to be changed/corrected as necessary. [Currently chcon -Rt svirt_sandbox_file_t ]
