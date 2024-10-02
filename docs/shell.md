# Shell script wrapping

All of those flags can be a lot to remember, but we can add a few shell scripts to our repo to make things a little simpler to work with. Doing so will also make any documentation in the README for setting up the project more concise!

## Structure

I often create a `bin` directory in the root of my projects that I work on that can house any scripts or other things made to be executed. Some of the scripts that end-up in the `bin` directory might be one off scripts that don't need to run very often. For the scripts that get executed frequently during development, I will create a symlink in the root of the project to make it easier to use.

```shell
mkdir bin
```

## `bun` shell script

We can take the `docker run` command that we previously ran and just put it in shell script with a simple and easy to remember name.

Create the file:

```shell
touch bin/bun.sh
```

We'll want to make the shell script executable as well:

```shell
chmod +x bin/bun.sh
```

::: details Write the following contents to the file:

```bash
#!/bin/bash

docker run --rm -v ./:/home/bun/app -p 5173:5173 --user $UID -it oven/bun docs:dev

```

:::

We can change the script to pass arguments to the script through to the container's command use the `$@` special parameter

```diff
#!/bin/bash

-docker run --rm -v ./:/home/bun/app -p 5173:5173 --user $UID -it oven/bun docs:dev
+docker run --rm -v ./:/home/bun/app -p 5173:5173 --user $UID -it oven/bun $@

```

Now, you can simply run `./bin/bun.sh i` to install packages or `./bin/bun.sh docs:dev` without having to remember all of those flags

## Symlink

I mentioned before I like to create symlinks for commonly used shell scripts and executing `bun` commands or `package.json` scripts would definitely fall under that classification.

From the root of the directory, create a symlink to the shell script that was just created, with the symlink just called `bun`:

```shell
ln -s ./bin/bun.sh ./bun
```

Even simpler now, you can just call `./bun i` to install dependencies, which is not far off from what it would be like to have `bun` installed directly on your machine. The advantage to this is that the bun version is tied to the version of the `oven/bun` image in the script. This also means the version can be committed to source control!

// TODO: add docker pull