# UPNP Media Server

## Client 

- Denon HEOS AVR
- Bubble UPNP
- VLC (firestick, Android)

## Server

### QNAP multimedia station and media console streaming addons


### Cohen3

See repo https://github.com/opacam/Cohen3

Install from git (Twisted error otherwise when using pip delivery)

```shell
# https://github.com/pypa/get-pip
curl -sSL https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py
pip3 install --upgrade --user Twisted 
pip3 install --user https://github.com/opacam/Cohen3/archive/master.zip
```


```shell
cohen3 --plugin=backend:FSStore,content:/home/scoulomb/pCloudDrive/nas-sync-direct-rclone/Multimedia/Musics
```


or use config file

```shell
mkdir ~/tmp
curl -o  ~/tmp/desktop_config.cohen3 https://raw.githubusercontent.com/open-denon-heos/upnp-media-server/main/cohen3_config/desktop_config.cohen3
cohen3 -c ~/tmp/desktop_config.cohen3
```

Can Trigger as a daemon
```shell
# Create
mkdir ~/cohen-logs
nohup cohen3 -c ~/tmp/desktop_config.cohen3 > ~/cohen-logs/out 2>~/cohen-logs/errors&
echo $! > ~/tmp/save_cohen_pid.txt

# Clean 
kill -9 $(cat ~/tmp/save_cohen_pid.txt)
rm ~/tmp/save_cohen_pid.txt
```

We can start media server at start-up via `crontab -e`, add following line

````shell
@reboot /home/scoulomb/.local/bin/cohen3 -c ~/tmp/desktop_config.cohen3 > ~/cohen_out.xt  2>&1
````
<!--
Note the output directory help me to find cohen3 not found thus full path
-->

<!--
Doc:
https://stackoverflow.com/questions/525247/how-do-i-daemonize-an-arbitrary-script-in-unix
https://stackoverflow.com/questions/17385794/how-to-get-the-process-id-to-kill-a-nohup-process
-->
<!-- I use here path to pcloud which is sync (NAS -> Pcloud) -->


<!--
### Similarly, Cohen3 on a QNAP NAS to replace QNAP multimedia station



```shell
### Install python + pip, and cohen3 
# > go to appcenter and install/update Python 
ssh <user>@local.nas.coulombel.net
/opt/python3/bin/python3.10 -v
# It comes with pip
/opt/python3/bin/pip3.10 --help
/opt/python3/bin/pip3.10  install

mkdir ~/tmp # TMPDIR needed due to cache issue: https://github.com/pypa/pip/issues/5816 
TMPDIR=/share/homes/admin/tmp /opt/python3/bin/pip3.10 install  --cache-dir=/share/homes/admin/tmp --upgrade --user Twisted
TMPDIR=/share/homes/admin/tmp /opt/python3/bin/pip3.10 install  --cache-dir=/share/homes/admin/tmp --build /share/homes/admin/tmp --upgrade --user https://github.com/opacam/Cohen3/archive/master.zip
```

=> Failed due to disk space issue

In config file we would have done 

```
content = /share/homes/admin/scoulomb-data/Multimedia/Musics  # use find . to find folder on the NAS
```

### Tried via Docker (for laptop or NAS setup)

```
FROM python:3.9.7

RUN pip install --upgrade Twisted
RUN pip install https://github.com/opacam/Cohen3/archive/master.zip

WORKDIR /working_dir
COPY ./.cohen3 /working_dir

RUN cohen3

ENTRYPOINT ["/root/.local/bin/cohen3", "-c", "~/working_dir/.cohen3"]
```

````shell
docker build . -t cohen3
docker run --network host -v /home/scoulomb:/media_path  cohen3 
````
We have an error

```
builtins.AttributeError: 'Section' object has no attribute 'upper'
```

-->