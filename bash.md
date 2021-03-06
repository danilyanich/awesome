# Bash

A collection of handy bash commands, scripts and tips.



## Copy files from one machine to another in trusted network

If you completely trust everyone in the network and you can connect a port of the destination machine directly, you can use netcat.

On destination:
```sh
nc -l -p port 0.0.0.0 | tar zxvf - -C destination_directory
```

On source:
```sh
tar zxcf - filename | nc destination_ip port
```

This is the fastest possible way to send a file from one computer to another using digital networks



## Determine machine IP address that is currently used for internet connections

```sh
ip route get 8.8.8.8 | awk '{print $NF; exit}'
```

You will get machine's IP address that would be used to connect with the `8.8.8.8` address.
This is the Google's public DNS resolver.



## Determine machine external IP address for HTTP connections

```sh
curl -s http://whatismyip.akamai.com/ && echo
```

Make a request to [whatismyip.akamai.com](http://whatismyip.akamai.com/) and add trailing endline for god sake.
- `curl -s` - Suppress progress indicator.



## Establish SSH tunnel with port forwarding from local machine to remote machine

```sh
ssh -nNT -L local_port:remote_host:remote_port username@remote_address
```

Establish port forwarding from `localhost:local_port` on local machine to `remote_host:remote_port` on remote machine. Flags `-nNT` tells ssh not to allocate new tty on remote machine.



## Safer bash scripts

Put the following line to the beginning of your bash script:

```sh
set -euxo pipefail
```

The bash shell comes with several builtin commands for modifying the behavior of the shell itself. `set` command has several options that will help us write safer scripts.

- `set -e` - Exit immediately when a command fails.
- `set -o pipefail` - Set the exit code of a pipeline to that of the rightmost command to exit with a non-zero status, or to zero if all commands of the pipeline exit successfully.
- `set -u` - Treat unset variables as an error and exit immediately.
- `set -x` - Print each command before executing it.



## Quick folder navigation

Regular `cd` can get cumbersome if you want to go to a folder, run a script, and come back to where you are.

```sh
pushd directory_path
popd
```

`pushd` allows you to navigate to a `directory_path`, do work there, and then `popd` brings you back to where you were originally without having to remember the path back there.



## Repeat last command with sudo

When you’re trying to execute command which requires privileges, but you forgot `sudo`. Instead of retyping it all out use:
```sh
sudo !!
```

`!!` repeats your previous command.



## Connect to remote docker daemon

```sh
ssh -nNT -L 2375:/var/run/docker.sock <ssh connection>
export DOCKER_HOST=tcp://localhost:2375
```
