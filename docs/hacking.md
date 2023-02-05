# Hacking a commercial smart TV

A commercial smart TV already contains the required hardware, operating system and device drivers for realizing a maker's smart TV. As all digital systems, commercial smart TV's can be tinkered with to meet some of the requirements of the project. Tinkering methods can be divided in two main categories:

1. Fencing the smart TV so that it cannot reach internet locations the TV manufacturer needs for services the user does not approve of
2. Rooting the smart TV and disabling all services the user does not approve of

## Fencing
The crudest possible measure for fencing a commercial smart TV is not giving it internet access (or only during initial setup if required). This will surely ban all advertizing and monitoring and provide the user with a good old dumb TV at a bargain price. Still beware of [TV types that persist in bothering the user to configure the network](https://eu.community.samsung.com/t5/tv/network-notification/td-p/4563968).

A more subtle type of fencing is to prevent the smart TV to fetch real-time advertizements from the internet. While this can be realized in several ways, two ones stick out in popularity:

* Configure the dns server settings in the smart TV to [point to AdGuard](https://adguard-dns.io/en/public-dns.html)
* Install a so-called [PiHole service](https://pi-hole.net/) in the home network and configure the dns server settings in the smart TV to point to this PiHole service.

This will prevent advertizements from being played by the firmware of current smart TV's, but there is no guarantee it will still work that way after the next firmware update. At least, these solutions seem to have problems in keeping there blacklists up-to-date for advertizers using larger auction platforms for advertizing space, cq YouTube.


## Rooting
There does not exist a general method for getting root access to the operating system of a commercial smart TV. Getting root access can only be done by exploiting vulnerabilities in the security measures of the TV. Such vulnerabilities depend on the TV at hand and on its firmware version. To get an idea of this, e.g. study the documentation of the [SamyGO project](http://wiki.samygo.tv/index.php).

Note that a smart TV is an applicance rather than a general computer system. This implies that the TV's firmware both includes the operating system - [often with an open source kernel and user space modules](https://github.com/vitalets/awesome-smart-tv) - and all device drivers, which are mostly proprietary.

Although rooting a commercial smart TV might be a rewarding project for a maker, it does not meet the project goal of realizing a system with long term support. In particular, rooting the device implies loosing the ability to receive firmware updates from the manufacterer (this would undo the rooting). This on its turn implies that the software of the system ages and the number of vulnerabilities unavoidably increases. So, if you want to pursue the approach of rooting a smart TV, do it in a responsible way such that it can not harm others. In particular:

* put the rooted device in an isolated VLAN of your home network
* configure the router of your home network such that the rooted device can only reach the internet addresses it needs (such as addresses of video streaming service providers, appstores, OS updates repositories, etc.)

Compare these terms on the terms for obtaining a firearm license. A rooted device can easily become an instrument of malware and create havoc on the internet.
