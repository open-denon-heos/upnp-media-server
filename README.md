# UPNP Media Server

## Client 

- Denon HEOS AVR
- Bubble UPNP
- VLC (firestick, Android)

## Server

### QNAP multimedia sation 

### Cohen3

See repo https://github.com/opacam/Cohen3

#### Install


Install from git 

```shell
pip3 install --upgrade --user Twisted pip3 install --user https://github.com/opacam/Cohen3/archive/master.zip
```


```shell
cohen3 --plugin=backend:FSStore,content:/home/scoulomb/pCloudDrive/nas-sync-direct-rclone/Multimedia/Musics
```


or use config file

```shell
cohen3 -c desktop_config.cohen3
```


