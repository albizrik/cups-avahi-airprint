# albizrik/cups-avahi-airprint

Fork from [chuckcharlie/cups-avahi-airprint](https://github.com/chuckcharlie/docker-cups-airprint) which was forked from [quadportnick/docker-cups-airprint](https://github.com/quadportnick/docker-cups-airprint)

This Alpine-based Docker image runs a CUPS instance with HPLIP drivers that is meant as an AirPrint relay for printers that are already on the network but not AirPrint capable. The other images out there never seemed to work right. I forked the original to use Alpine instead of Ubuntu and work on more host OS's.

albizrik: fixed a python import statement wich prevented the printer to be discovered by my iphone/ipad
tested and running on my QNAP Nas

## Configuration

### Volumes

* `/config`: where the persistent printer configs will be stored
* `/services`: where the Avahi service files will be generated

### Variables

* `CUPSADMIN`: the CUPS admin user you want created - default is lowercase cupsadmin if unspecified
* `CUPSPASSWORD`: the password for the CUPS admin user - default is admin username if unspecified

### Ports/Network

* Must be run on host network. This is required to support multicasting which is needed for Airprint.

## Running

### Example run command

```bash
docker run --name cups --restart unless-stopped  --net host\
  -v <your services dir>:/services \
  -v <your config dir>:/config \
  -e CUPSADMIN="<username>" \
  -e CUPSPASSWORD="<password>" \
  albizrik/cups-avahi-airprint:latest
```

## Add and set up printer

* CUPS will be configurable at http://[host ip]:631 using the CUPSADMIN/CUPSPASSWORD.
* Make sure you select `Share This Printer` when configuring the printer in CUPS.
* ***After configuring your printer, you need to close the web browser for at least 60 seconds. CUPS will not write the config files until it detects the connection is closed for as long as a minute.***


## Building

### If you want to build the files yourself

Build your image using the docker build command, where DH_ACC is your dockerhub account and DH_REPO is your dockerhub repo, and $TAG is the image tag.

```bash
docker build -t $DH_ACC/DH_REPO:$TAG . 
```

In my case:

```bash
docker build -t albizrik/cups-avahi-airprint:latest . 
```

### Push your files to dockerhub

Now, you can push this image to your hub by executing the docker push command.

```bsh
sudo docker push $DOCKER_ACC/$DOCKER_REPO:$TAG
```

In my case:

```bash
sudo docker push albizrik/cups-avahi-airprint:latest
```
