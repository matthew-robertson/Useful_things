1. By default, all AWS instances are in UTC. Info about changing the timezone can be found [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html).
1. After resizing the disk, you need to manually re-partition the disk, which AWS details [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html). The basic steps are:
    1. `sudo growpart /dev/xvda 1`
    1. `sudo xfs_growfs -d /`
