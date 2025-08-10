# Clean Up Docker

Docker is great but some of the downloaded images and containers can take up a lot of space on your computer. Some of these images might be for older projects that you don't even develop on anymore, therefore there's no need to have the image locally. If for whatever reason you need to go back to that project docker allows you to easily download any images that aren't found locally.

## Find Docker Usage
Using the command

```
$ docker system df
```

You can see how much space is being used by your images, container and volumes.

```
$ docker system df

TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              23                  9                   2.62GB              2.007GB (76%)
Containers          24                  1                   28.71MB             28.71MB (100%)
Local Volumes       71                  6                   1.217GB             1.216GB (99%)
Build Cache         0                   0                   0B                  0B
```

As you can see from the above there are 3.2GB that can be reclaimed.

## Docker System Prune
This command is a shortcut that can prune the images, containers and networks.
```
$ docker system prune

WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
```

This will remove all the containers that aren't in used and all active volumes but there will still be a lot of times left over that might not be in use of any containers.

## Docker Image Prune
To delete all images without at least one container associated with them you can use the command `docker image prune -a`

```
$ docker image prune -a

WARNING! This will remove all images without at least one container associated to them.
Are you sure you want to continue? [y/N] y
```

If you now run the command `docker system df` you'll see that the only images you'll have locally are the active images.

## Docker Container Prune
If you want to just remove any stopped container then you can do so by using the container prune command

```
$ docker container prune

WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
```

You can even use a `--filter` flag that will allow you to only delete containers stopped within a certain time period.

```
$ docker container prune --filter "until=24h"
```

## Docker Volume Prune
If you would like to remove the unused volumes then you can use the command `docker volume prune`.

```
$ docker volume prune

WARNING! This will remove all volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
```

After doing all the above you can return to the `docker system df` command to save how much space you have saved.
