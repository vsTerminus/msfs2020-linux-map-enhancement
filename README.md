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

In `extra/server/`:

    pip -r requirements.txt

Or if you are on Arch Linux it will complain that you should use pacman instead. You can find everything in the official or AUR repos except for `pyGeoTile`.
I chose to install this with pip and override the warning:

    pip install --user --break-system-packages pyGeoTile

## mkcert

This is required for nginx, it creates a local self-signed certificate for your webserver to use.

You should be able to find this in your distro's package manager, eg

    sudo pacman -Sy mkcert


## NGINX

So far I haven't found a good way to launch nginx from the application (with sudo), so for now my solution is to launch it separately.

You have two options:

1. You can run the packaged `nginx.exe` with `wine`

    cd extra/nginx
    sudo wine nginx.exe

2. You can install nginx with your system package manager and then just manually start it and point it at the packaged config file

    sudo pacman -Sy nginx
    sudo nginx -c $PWD/extra/nginx/conf/nginx.conf

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

# Troubleshooting

## Nginx Server failed

Right now the app won't even try to launch nginx, so you'll have to launch it yourself. See the NGINX section above.

## Warnings

If you are already running NGINX you should see a warning when you launch the app:

    Port 443 in use - Application will not work (Unless you are running your own NGINX)

If you are *not* running your own NGINX and you see this warning it means you have something else binding that port and will need to figure out what it is.
