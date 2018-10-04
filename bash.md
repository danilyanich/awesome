# Bash



## Copy files from one machine to another in trusted network

If you completely trust everyone in the network and you can connect a port of the destination machine directly, you can use netcat.

On destination:
```bash
nc -l -p port 0.0.0.0 | tar zxvf - -C destination_directory
```

On source:
```bash
tar zxcf - filename | nc destination_ip port
```

This is the fastest possible way to send a file from one computer to another using digital networks



## Determine machine IP address that is currently used for internet connections

```bash
ip route get 8.8.8.8 | awk '{print $NF; exit}'
```

You will get machine's IP address that would be used to connect with the `8.8.8.8` address.
This is the Google's public DNS resolver.



## Determine machine external IP address for HTTP connections

```bash
curl -s http://whatismyip.akamai.com/ && echo
```

Make a request to [whatismyip.akamai.com](http://whatismyip.akamai.com/) and add trailing endline for god sake.



## Establish SSH tunnel with port forwarding from local machine to remote machine

```bash
ssh -nNT -L local_port:remote_host:remote_port username@remote_address
```

Establish port forwarding from `localhost:local_port` on local machine to `remote_host:remote_port` on remote machine. Flags `-nNT` tells ssh not to allocate new tty on remote machine.