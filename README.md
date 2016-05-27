docker-squash
=============

This works exactly like the native version of `docker-squash` so familiarize yourself with it's docs at https://github.com/jwilder/docker-squash.

The main difference is you'll need to swap out the `sudo docker-squash` command for `docker run -v /tmp -i myyk/docker-squash` and optionally add volumes for you inputs/outputs.

So if before you had:

    $ docker save <image id> > image.tar
    $ sudo docker-squash -i image.tar -o squashed.tar
    $ cat squashed.tar | docker load
    $ docker images <new image id>

Now you'll have:

    $ docker save <image id> > input/image.tar
    $ docker run -v $(pwd)/input:/input -v $(pwd)/output:/output -v /tmp -i myyk/docker-squash -i input/image.tar -o output/squashed.tar
    $ cat output/squashed.tar | docker load
    $ docker images <new image id>

I've confirmed this works from OS X (Yosemite) with `docker-machine` and from Ubuntu (Trusty).

# Setup the alias. Put this in your .bash_rc or .zshrc file so it's available at startup.
alias squash="docker run -v $(pwd)/input:/input -v $(pwd)/output:/output -v /tmp -i squash:1st"

$ squash docker-squash -i /input/image.tar -o /output/squashed.tar -verbose
$ squash tar --delete -f output/squashed.tar manifest.json
