# omar-web-proxy

The OMAR Web Proxy application acts as a proxy and reverse proxy for the OMAR services.  This is a generic web proxy and for full installation instructions please see:

- [install-guide](docs/install-guide/omar-web-proxy.md)

## Building omar-web-proxy

You must checkout both omar-common and omar-web-proxy: 

- git clone https://github.com/ossimlabs/omar-common
- git clone https://github.com/radiantbluetechnologies/omar-web-proxy
- `export OMAR_COMMON_PROPERTIES=<location of omar-common/omar-common-properties.gradle`
- gradle doAll


This will create and build the docker image and install onto our registry and copy the image to our delivery directory on S3.



