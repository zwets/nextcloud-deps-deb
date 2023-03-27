# nextcloud-deps-deb

Creates a Debian/Ubuntu package `nextcloud-deps` that depends on the
Apache2 and PHP packages required for a NextCloud installation.

## Building

Install the Debian `equivs` package that will build the deb package:

    sudo apt install equivs

Build the package:

    equivs-build control

## Installing

Install the package as usual:

    sudo apt install ./nextcloud-deps_{VERSION}.deb

## Apache2 configuration

Set up for PHP, but do not enable it by default:

    sudo a2dismod php8.1 mpm_prefork
    sudo a2enmod mpm_event proxy_fcgi setenvif

Instead enable php per virtual host:

    # Inside the VirtualHost definition
    Include conf-available/php8.1-fpm.conf

Activate the requisite modules for NextCloud

    sudo a2enmod rewrite headers env dir mime

Then follow the NextCloud instructions
    
## Updating this deb

To add a new dependency add it to the Depends or Recommends line in
the `control` file.

Then update the `changelog` file, making sure you bump the version
number:

    dch -c changelog -v {NEW_VERSION}

Then rebuild.  Note that the package version is taken from the top
changelog entry.

