# Apache Cloud Drive

Apache Cloud Drive is a app for storing your files on some kind of ARM computer (ex. Rasberry PI hidden under your TV in a living room).

Under the hood it's a Apache HTTP server with webDAV module and SSL cetificate supplied.

## Requirements
Install docker, docker-compose and apache2-utils.
Docker installation instructions are available at: https://docs.docker.com/install/linux/ubuntu/ and 
https://docs.docker.com/compose/install

As mentioned earlier Apache Cloud Drive generate SSL certifate. Certificate is provided by Let's Encrypt so it's necessary to make your machine available for Let's Encrypt verification servers, ie. Apache Cloud Drive host machine have to have public and static IP address and ports 80 and 443 open.

Last but not least Apache Cloud Drive requires registered public domain.

If your ISP doesn't provide static IP or you don't have any registered domain you should try https://www.noip.com/.

## Setup
Open `docker-compose.yml` and replace:
* `<YOUR_DOMAIN>` with your domain
* `<YOUR_EMAIL>` with your email
* `<YOUR_STORAGE>` with location of your storage, ie. place where you want to store your files.

Create `.htpasswd` with user credentials by running:
`htpasswd <YOUR_STORAGE>/.htpasswd <USERNAME>` and typing password.

## Run
To start it execute `docker-compose up -d` inside directory with `docker-compose.yml`

## Testing 
Lets Encrypt defines some limits and rates for number of issued certificates and failed challenges (https://letsencrypt.org/docs/rate-limits) so if you are just playing around you may want to use test mode: set `LETSENCRYPT_TEST=true` in `docker-compose.yml`.
