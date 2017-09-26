# Audiostation Docker

Network Audio Player with  [Mopidy](https://www.mopidy.com/),[SnapCast](https://github.com/badaix/snapcast) and [upmpdcli](https://www.lesbonscomptes.com/upmpdcli/).

Use [tkem/mopidy-dleyna](https://github.com/tkem/mopidy-dleyna) to load the Music from external Devices with [DLNA support](https://01.org/dleyna).  
Use [UPnP](https://wikipedia.org/wiki/Universal_Plug_and_Play) for receiving the Music from Mobile Devices ([BubbleUPnP](https://play.google.com/store/apps/details?id=com.bubblesoft.android.bubbleupnp)) or other clients.
Use a custome [Mopidy plugin](https://github.com/nolte/mopidy-mongodb-playlist) to got a central Store for created Playlists. Saved in a MongoDB instanze.

I am using a [HiFiBerry Digi+](https://www.hifiberry.com/products/digiplus/) as additional sound card for my RPI2. The RPI are connected with [TOSLink](https://en.wikipedia.org/wiki/TOSLINK) to my Denon Audio Revceiver

To Start the docker Contaieners on RPI i use [hypriot](https://blog.hypriot.com/) as base System image.

## Starting

Use for starting use on **RPI**
```
docker-compose -f docker-compose-rpi.yml up
```

and on classic **amd64** Systems
```
docker-compose -f docker-compose.yml up
```

after the containers are running you can open [http://localhost:6680](http://localhost:6680) and use the Mopdiy webfrontend.

**don`t forget** Mopidy has a [HTTP JSON-RPC API](https://docs.mopidy.com/en/latest/api/http/) and can be integrated to [home-assistant.io](https://home-assistant.io/components/media_player.mpd/).

**Tested Android UPnP Apps**

- [BubbleUPnP](https://play.google.com/store/apps/details?id=com.bubblesoft.android.bubbleupnp),  works so good that i bought the Full Version, and did not test more apps.

## Structure

You can find the used Dockerfiles at the GitHub repositories: [docker-snapcast](https://github.com/nolte/docker-snapcast), [docker-mopidy](https://github.com/nolte/docker-mopidy) and [docker-upmpdcli](https://github.com/nolte/docker-upmpdcli)

#### Used Dockerimages

The Connection between the Moidy Container and the [SnapCast Server](https://github.com/badaix/snapcast) runs over a gstreamer fifo file, configured as output in Mopidy.

```
...
[audio]
output = audioresample ! audioconvert ! audio/x-raw,rate=48000,channels=2,format=S16LE ! wavenc ! filesink location=/tmp/sharesound/snapfifo
...
```
form [snapcast player setup ](https://github.com/badaix/snapcast/blob/master/doc/player_setup.md#mopidy)


On Docker the fifo file will be create by a data container, all three containers share a [volume](https://docs.docker.com/compose/compose-file/#volumes) which contains the shared file.

##### Docker Hub Used Images

**amd64**
- https://hub.docker.com/r/nolte/snapcast-server/
- https://hub.docker.com/r/nolte/snapcast-client/
- https://hub.docker.com/r/nolte/mopidy/
- https://hub.docker.com/r/nolte/upmpdcli/
- https://hub.docker.com/_/mongo/

**armhf**
- https://hub.docker.com/r/nolte/rpi-snapcast-server/
- https://hub.docker.com/r/nolte/rpi-snapcast-client/
- https://hub.docker.com/r/nolte/rpi-mopidy/
- https://hub.docker.com/r/nolte/rpi-upmpdcli/
- https://hub.docker.com/r/nolte/rpi-mongo/
