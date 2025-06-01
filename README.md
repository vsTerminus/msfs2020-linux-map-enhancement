# msfs2020-map-enhancement
MSFS2020 Map Enhancement

This is MSFS2020 Map Enhancement v5.0.9
After this version the source was closed

This fork exists to get the application running on Linux.

Please note: This version does not have any of the new features developed by the original author for versions 6 and 7. Things may not work correctly (or at all). Be prepared for things to not "just work" the first time and require some tinkering.

# Install Dependencies

## NPM

In the root directory:

        npm install

## Python

Take a look in `extra/server/requirements.txt`. You will need to install all of the packages listed there.

You can do this one of two ways:

1. If you can find them, install each package via your distro's package manager.
2. If you can't, activate a python venv and run `pip -r requirements.txt` from the `extra/server/` directory.

In my experience you shouldn't have any trouble finding the packages, except for `pyGeoTile`.

If you can't find it and don't want to use a venv you can override your package manager and install this via pip with:

        pip install --user --break-system-packages pyGeoTile


## NGINX

On Windows this app launches its own instance of nginx which binds to 443 and listens for requests for map tiles from the game.

On Linux that's both technically challenging to do (443 is a protected port and requires root to bind) and also not really the way things are done.

You have two options:

1. You can run the packaged `nginx.exe` with `wine`

        cd extra/nginx
        sudo wine nginx.exe

2. You can install nginx via your package manager and use the included config file (Found in `extra/nginx/conf/nginx.conf`)

Both options seemed to work for me. I prefer the second one.
Note: If you see errors about missing directories you may have to create them.

# Hosts File

I'm not running this app with `sudo` but modifying the `/etc/hosts` file would require that. So my solution for now has just been to point the app at a dummy file and modify `/etc/hosts` myself.

1. In the project root directory (or in the `linux-unpacked` directory if you choose to build the project) create an empty file named `hosts`

        touch hosts

2. Edit your actual `/etc/hosts` file and add the following lines:
 
        127.0.0.1 kh.ssl.ak.tiles.virtualearth.net
        127.0.0.1 khstorelive.azureedge.net

Obviously the downside here is that you'll have to manually remove/comment these out when you're not playing.

# Run

The simplest way to get the application running is

        npm run dev

You can also build the source into a packaged executable:

        npm run build

Then you will find two versions:

1. A fully packaged app in `release/MSFS2020 Map Enhancement-5.0.9.AppImage`
2. An unpacked app in `release/linux-unpacked`

I've had better luck getting it to run correctly from the `linux-unpacked` folder. (The AppImage binary seems to fail on mkcert)

Once the app launches click "Start Flying" and you should see "Image Server" and "Nginx Server" status both change to "good" at the bottom. You're all set.