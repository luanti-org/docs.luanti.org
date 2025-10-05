---
title: Remote media
aliases:
  - /Remote_media
  - /server/remote-media
---

# Remote media

When connecting to a server, Luanti allows for media to be received over a remote HTTP server, as opposed to "traditionally" over the UDP server connection (assuming Luanti is compiled with cURL support and `enable_remote_media_server` is enabled).

Usually this is a more efficient means of transferring for servers with large amounts of media, even if you host the HTTP server on the same computer as the Luanti server it should still be able to download faster. It is also useful for Luanti servers hosted on a low speed residential internet connection, where the media downloading can be offloaded to make it significantly faster and dedicate more of the connection to communication with the Luanti server.

If a media file required on the server doesn't exist on the remote server or the media server can't be connected to for some reason, it will fall back to the traditional transfer method. This also applies if cURL support or the `enable_remote_media_server` setting is disabled.

## Technical info

**In version 5.11.0 or before:**  
At first, the client will POST request `[remote_media]index.mth`, which contains the list of (SHA1) hashes that are available on the server. The client will then check what hashes it wants, but does not currently have cached, and requests `[remote_media][SHA1 hash]` for each one if it exists in the hash index. If it doesn't exist, it will fall back to traditional transfer.

**As of version 5.12.0:**  
The client request to `[remote_media]index.mth` [was changed](https://github.com/luanti-org/luanti/pull/15885) to a GET request to facilitate using static hosting for remote media

Preferably the server should support HTTP/2, though unfortunately the official Android and Windows builds do not come with a cURL linked against libnghttp2 currently. Linux however will usually be linked against a cURL system library that is linked against libnghttp2.

## Setting a remote media server

Set the `remote_media` setting to the URL of the media server. Restart.

## Hosting a remote media server

This section goes over hosting a remote media server statically, using nginx as the HTTP server.

First of all you need to populate the list of media and create an `index.mth` file, which contains a list of hashes that exist on the media server. This is a shell script you could run e.g. inside of a game's folder or a folder of mods:

{{% comment %}} cspell:disable {{% /comment %}}

```bash
#!/bin/bash

collect_from () {
	echo "Processing media from: $1"
	find -L "$1" -type f -name "*.png" -o -name "*.jpg" -o -name "*.jpeg" -o -name "*.ogg" -o -name "*.x" -o -name "*.b3d" | while read f; do
		basename "$f"
		hash=`openssl dgst -sha1 <"$f" | cut -d " " -f 2`
		cp "$f" media/$hash
	done
}

mkdir -p media/
# Change this 'collect_from' or add more lines of 'collect_from', the script will recursively
# search for files with extensions of Luanti media in this folder.
# This is an example to be run in a game folder, but you can change this to anything to catch
# all textures that a server uses.
collect_from mods/
# Example for MineClone2 (they put textures in textures/ now)
#collect_from mods/
#collect_from textures/

printf "Creating index.mth... "
printf "MTHS\x00\x01" > media/index.mth
find media/ -type f -not -name index.mth | while read f; do
	openssl dgst -binary -sha1 <$f >> media/index.mth
done
```

{{% comment %}} cspell:enable {{% /comment %}}

You should now have a folder called `media` with a bunch of hashed filenames and an `index.mth` file. This can then be moved into a folder for nginx to host.

Unfortunately nginx does not allow POST requests on static files, so to make nginx able to send back the `index.mth` file which gets requested with POST you need to add this to the configuration for the site:

```nginx
error_page 405 =200 $uri;
```

## See Also

- [sfan5's remote media build script](https://gist.github.com/sfan5/6351560)
- [Forum topic with comments](https://forum.luanti.org/viewtopic.php?f=3&t=9260)
- [MtMediaServer](https://forum.luanti.org/viewtopic.php?f=14&t=17411)
- [mt_media_collector](https://github.com/ShadowNinja/mt_media_collector)
