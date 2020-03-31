# The Filter Node Errors out with «libproton error: did not allocate memory» 

    #### Warning:: INSTRUCTION DEPRECATED
    This issue is irrelevant for the latest filter node version users.

See the full error message in the log file:

```
nginx: the configuration file /etc/nginx-wallarm/nginx.conf syntax is ok
nginx: libproton error: did not allocate memory
nginx: wallarm enabled but wallarm training set file /etc/wallarm/lom is invalid, please check wallarm_local_trainingset_path
nginx: configuration file /etc/nginx-wallarm/nginx.conf test failed
```

The error cause is in that the shared memory is smaller than the LOM file.

Increase the shared memory value to make it bigger than the LOM file:

1. Open for editing the configuration file in `/etc/nginx-wallarm`.
2. Set the `wallarm_shm_size` value bigger than the size of the file `/etc/wallarm/lom`.
