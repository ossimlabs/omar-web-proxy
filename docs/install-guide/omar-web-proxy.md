# OMAR Web Proxy

## Purpose

The OMAR Web Proxy application serves as an entry point for routing and PKI enablement of all OMAR services. Though there are many ways to provide routing and security, the OMAR web proxy provides a "one stop shop" for the OMAR services. We have added example for doing a simple password protected endpoint(s) that is conditional based on an htpasswd file existing on disk.

##Configuration

The web service has the certificates and the certificate of authority  located under the directory **/etc/ssl/server-certs**. The files created are named **server.key** and **server.pem** and **ca.crt**.  

The apache conf.d directory contains several conf file definitions.  Our **reverse-proxy.conf** file for apache contains the reverse proxy, CORS support, and the Virtual Host SSL definitions:

```
##################################
# o2 Openshift Proxy configurations
##################################

ProxyPreserveHost On

Redirect /ngt-service /ngt-service/
ProxyPass /ngt-service/ http://ngt-service:8080/ngt-service/
ProxyPassReverse /ngt-service/ http://ngt-service:8080/ngt-service/

Redirect /omar /omar/
ProxyPass /omar/ http://omar-oldmar-app:8080/omar/
ProxyPassReverse /omar/ http://omar-oldmar-app:8080/omar/

ProxyPass /omar-admin-server http://omar-admin-server:8080/omar-admin-server
ProxyPassReverse /omar-admin-server http://omar-admin-server:8080/omar-admin-server

Redirect /omar-cesium-terrain-builder /omar-cesium-terrain-builder/
ProxyPass /omar-cesium-terrain-builder/ http://omar-cesium-terrain-buil:8080/omar-cesium-terrain-builder/
ProxyPassReverse /omar-cesium-terrain-builder/ http://omar-cesium-terrain-buil:8080/omar-cesium-terrain-builder/

Redirect /omar-cesium-terrain-server /omar-cesium-terrain-server/
ProxyPass /omar-cesium-terrain-server/ http://omar-cesium-terrain-serv:8000/
ProxyPassReverse /omar-cesium-terrain-server/ http://omar-cesium-terrain-serv:8000/

Redirect /omar-docs /omar-docs/
ProxyPass /omar-docs/ http://omar-docs-app:80/omar-docs/
ProxyPassReverse /omar-docs/ http://omar-docs-app:80/omar-docs/

Redirect /omar-download /omar-download/
ProxyPass /omar-download/ http://omar-download-app:8080/omar-download/
ProxyPassReverse /omar-download/ http://omar-download-app:8080/omar-download/

Redirect /omar-dropbox-sqs /omar-dropbox-sqs/
ProxyPass /omar-dropbox-sqs/ http://omar-dropbox-sqs-app:8080/omar-dropbox-sqs/
ProxyPassReverse /omar-dropbox-sqs/ http://omar-dropbox-sqs-app:8080/omar-dropbox-sqs/

Redirect /omar-geoscript /omar-geoscript/
ProxyPass /omar-geoscript/ http://omar-geoscript-app:8080/omar-geoscript/
ProxyPassReverse /omar-geoscript/ http://omar-geoscript-app:8080/omar-geoscript/

Redirect /omar-git-mirror /omar-git-mirror/
ProxyPass /omar-git-mirror/ http://omar-git-mirror-app:8080/omar-git-mirror/
ProxyPassReverse /omar-git-mirror/ http://omar-git-mirror-app:8080/omar-git-mirror/

ProxyPass /omar-jpip http://omar-jpip-app:8080/omar-jpip
ProxyPassReverse /omar-jpip http://omar-jpip-app:8080/omar-jpip

Redirect /omar-mensa /omar-mensa/
ProxyPass /omar-mensa/ http://omar-mensa-app:8080/omar-mensa/
ProxyPassReverse /omar-mensa/ http://omar-mensa-app:8080/omar-mensa/

RedirectMatch ^/$ /omar-ui/omar#/map
Redirect /omar-ui /omar-ui/
ProxyPass /omar-ui/ http://omar-ui-app:8080/omar-ui/
ProxyPassReverse /omar-ui/ http://omar-ui-app:8080/omar-ui/

Redirect /omar-sqs-stager /omar-sqs-stager/
ProxyPass /omar-sqs-stager/ http://omar-sqs-stager-app:8080/omar-sqs-stager/
ProxyPassReverse /omar-sqs-stager/ http://omar-sqs-stager-app:8080/omar-sqs-stager/

Redirect /omar-stager /omar-stager/
ProxyPass /omar-stager/ http://omar-stager-app:8080/omar-stager/
ProxyPassReverse /omar-stager/ http://omar-stager-app:8080/omar-stager/

Redirect /omar-superoverlay /omar-superoverlay/
ProxyPass /omar-superoverlay/ http://omar-superoverlay-app:8080/omar-superoverlay/
ProxyPassReverse /omar-superoverlay/ http://omar-superoverlay-app:8080/omar-superoverlay/

Redirect /omar-wcs /omar-wcs/
ProxyPass /omar-wcs/ http://omar-wcs-app:8080/omar-wcs/
ProxyPassReverse /omar-wcs/ http://omar-wcs-app/omar-wcs/

Redirect /omar-wfs /omar-wfs/
ProxyPass /omar-wfs/ http://omar-wfs-app:8080/omar-wfs/
ProxyPassReverse /omar-wfs/ http://omar-wfs-app:8080/omar-wfs/

Redirect /omar-wms /omar-wms/
ProxyPass /omar-wms/ http://omar-wms-app:8080/omar-wms/
ProxyPassReverse /omar-wms/ http://omar-wms-app:8080/omar-wms/

Redirect /omar-oms /omar-oms/
ProxyPass /omar-oms/ http://omar-oms-app:8080/omar-oms/
ProxyPassReverse /omar-oms/ http://omar-oms-app:8080/omar-oms/

Redirect /omar-wmts /omar-wmts/
ProxyPass /omar-wmts/ http://omar-wmts-app:8080/omar-wmts/
ProxyPassReverse /omar-wmts/ http://omar-wmts-app:8080/omar-wmts/

Redirect /service-proxy /service-proxy/
ProxyPass /service-proxy/ http://omar-service-proxy-app:8080/service-proxy/
ProxyPassReverse /service-proxy/ http://omar-service-proxy-app:8080/service-proxy/

Redirect /tlv /tlv/
ProxyPass /tlv/ http://tlv-app:8080/tlv/
ProxyPassReverse /tlv/ http://tlv-app:8080/tlv/

Redirect /twofishes /twofishes/
ProxyPass /twofishes/ http://omar-twofishes:8081/
ProxyPassReverse /twofishes/ http://omar-twofishes:8081/

Redirect /omar-avro-metadata /omar-avro-metadata/
ProxyPass /omar-avro-metadata/ http://omar-avro-metadata-app:8080/omar-avro-metadata/
ProxyPassReverse /omar-avro-metadata/ http://omar-avro-metadata-app:8080/omar-avro-metadata/

Redirect /omar-mapproxy /omar-mapproxy/
ProxyPass /omar-mapproxy/ http://omar-mapproxy:8080/
ProxyPassReverse /omar-mapproxy/ http://omar-mapproxy:8080/

Redirect /omar-turbine-server /omar-turbine-server/
ProxyPass /omar-turbine-server/ http://omar-turbine-server:8989/omar-turbine-server/
ProxyPassReverse /omar-turbine-server/ http://omar-turbine-server:8989/omar-turbine-server/

Redirect /isa-ui https://pki-omar-dev.ossim.io/isa-ui
Redirect /isa-ui/ https://pki-omar-dev.ossim.io/isa-ui/
Redirect /ossim-isa https://pki-omar-dev.ossim.io/ossim-isa
Redirect /ossim-isa/ https://pki-omar-dev.ossim.io/ossim-isa/


#######################################
# VirtualHost configuration for SSL
#######################################
<VirtualHost _default_:443>
  SSLEngine ON

  SSLCertificateFile /etc/ssl/server-certs/server.pem
  SSLCertificateKeyFile /etc/ssl/server-certs/server.key

  SSLCACertificateFile /etc/ssl/server-certs/ca.crt

  SSLVerifyClient none
  SSLVerifyDepth 10

</VirtualHost>

################################################
# Protect locations if the htpasswd file exists
################################################
<Location /omar-stager/dataManager/removeRaster>
    <If "-f '/etc/httpd/htpasswd/htpasswd'">
      AuthType Basic
      AuthName "Wrapper auth"
      AuthBasicProvider file
      AuthUserFile "/etc/httpd/htpasswd/htpasswd"
      Require valid-user
    </If>
</Location>

#######################################
# CORS configuration
#######################################

# Allow these headers to vary in caches
Header always append Vary Access-Control-Allow-Credentials
Header always append Vary Access-Control-Allow-Methods
Header always append Vary Access-Control-Allow-Headers
Header always append Vary Access-Control-Allow-Origin
Header always append Vary Access-Control-Max-Age

# Inform the client CORS headers will vary based on the provided Origin
Header always append Vary Origin

# For proxypass
Header always unset Access-Control-Allow-Credentials
Header always unset Access-Control-Allow-Origin

# Header always set Access-Control-Allow-Credentials true
Header set Access-Control-Allow-Credentials true

# This is a pretty standard set, update as needed for custom requests
Header always set Access-Control-Allow-Methods "GET, PUT, POST, DELETE, OPTIONS, CANCELUPLOAD"
Header always set Access-Control-Allow-Headers "Accept, Overwrite, Destination, Content-Type, Cache-Control, Origin, Depth, User-Agent, X-File-Size, X-Requested-With, If-Modified-Since, X-File-Name"

# This grabs the origin from the request headers and echoes it in the Allow-Origin header
# so we can support all origins without a wildcard

SetEnvIfNoCase Origin (.*) origin=$1
# Header always set Access-Control-Allow-Origin "%{origin}e"
Header set Access-Control-Allow-Origin "%{origin}e"

# This should be pretty short in the event that the user submits a cross-origin request
# from two separate origins over a short period of time. If the browser denying these
# within a short period is not acceptable, see the commented block below.
# Header always set Access-Control-Max-Age 30

# The following conditions are here to satisfy not responding to older http requests (per NESSUS 1.5.9)
RewriteEngine on
RewriteCond %{THE_REQUEST} !HTTP/1\.1$
RewriteRule .* - [F]
# Add these rewrite directives before any ProxyPass or ProxyPassReverse load balancing directives
# so the pre-flight request does not go to the balancer handler.
RewriteCond %{REQUEST_METHOD} OPTIONS
RewriteCond %{HTTP:Origin} !=""
RewriteRule ^(.*)$ $1 [R=200,L,E=HTTP_ORIGIN:%{HTTP:Origin}]

# Allow browsers to access additional response headers for cross-domain requests.
Header always set Access-Control-Expose-Headers "Content-Disposition"
```

For completeness we wil include the conf files that come when we install mod_ssl and the apche server that are found under the conf.d directory.


**autoindex.conf**:

```
#
# Directives controlling the display of server-generated directory listings.
#
# Required modules: mod_authz_core, mod_authz_host,
#                   mod_autoindex, mod_alias
#
# To see the listing of a directory, the Options directive for the
# directory must include "Indexes", and the directory must not contain
# a file matching those listed in the DirectoryIndex directive.
#

#
# IndexOptions: Controls the appearance of server-generated directory
# listings.
#
IndexOptions FancyIndexing HTMLTable VersionSort

# We include the /icons/ alias for FancyIndexed directory listings.  If
# you do not use FancyIndexing, you may comment this out.
#
Alias /icons/ "/usr/share/httpd/icons/"

<Directory "/usr/share/httpd/icons">
    Options Indexes MultiViews FollowSymlinks
    AllowOverride None
    Require all granted
</Directory>

#
# AddIcon* directives tell the server which icon to show for different
# files or filename extensions.  These are only displayed for
# FancyIndexed directories.
#
AddIconByEncoding (CMP,/icons/compressed.gif) x-compress x-gzip

AddIconByType (TXT,/icons/text.gif) text/*
AddIconByType (IMG,/icons/image2.gif) image/*
AddIconByType (SND,/icons/sound2.gif) audio/*
AddIconByType (VID,/icons/movie.gif) video/*

AddIcon /icons/binary.gif .bin .exe
AddIcon /icons/binhex.gif .hqx
AddIcon /icons/tar.gif .tar
AddIcon /icons/world2.gif .wrl .wrl.gz .vrml .vrm .iv
AddIcon /icons/compressed.gif .Z .z .tgz .gz .zip
AddIcon /icons/a.gif .ps .ai .eps
AddIcon /icons/layout.gif .html .shtml .htm .pdf
AddIcon /icons/text.gif .txt
AddIcon /icons/c.gif .c
AddIcon /icons/p.gif .pl .py
AddIcon /icons/f.gif .for
AddIcon /icons/dvi.gif .dvi
AddIcon /icons/uuencoded.gif .uu
AddIcon /icons/script.gif .conf .sh .shar .csh .ksh .tcl
AddIcon /icons/tex.gif .tex
AddIcon /icons/bomb.gif /core
AddIcon /icons/bomb.gif */core.*

AddIcon /icons/back.gif ..
AddIcon /icons/hand.right.gif README
AddIcon /icons/folder.gif ^^DIRECTORY^^
AddIcon /icons/blank.gif ^^BLANKICON^^

#
# DefaultIcon is which icon to show for files which do not have an icon
# explicitly set.
#
DefaultIcon /icons/unknown.gif

#
# AddDescription allows you to place a short description after a file in
# server-generated indexes.  These are only displayed for FancyIndexed
# directories.
# Format: AddDescription "description" filename
#
#AddDescription "GZIP compressed document" .gz
#AddDescription "tar archive" .tar
#AddDescription "GZIP compressed tar archive" .tgz

#
# ReadmeName is the name of the README file the server will look for by
# default, and append to directory listings.
#
# HeaderName is the name of a file which should be prepended to
# directory indexes.
ReadmeName README.html
HeaderName HEADER.html

#
# IndexIgnore is a set of filenames which directory indexing should ignore
# and not include in the listing.  Shell-style wildcarding is permitted.
#
IndexIgnore .??* *~ *# HEADER* README* RCS CVS *,v *,t
```
**ssl.conf***:

```
#
# When we also provide SSL we have to listen to the
# the HTTPS port in addition.
#
Listen 443 https

##
##  SSL Global Context
##
##  All SSL configuration in this context applies both to
##  the main server and all SSL-enabled virtual hosts.
##

#   Pass Phrase Dialog:
#   Configure the pass phrase gathering process.
#   The filtering dialog program (`builtin' is a internal
#   terminal dialog) has to provide the pass phrase on stdout.
SSLPassPhraseDialog exec:/usr/libexec/httpd-ssl-pass-dialog

#   Inter-Process Session Cache:
#   Configure the SSL Session Cache: First the mechanism
#   to use and second the expiring timeout (in seconds).
SSLSessionCache         shmcb:/run/httpd/sslcache(512000)
SSLSessionCacheTimeout  300

#   Pseudo Random Number Generator (PRNG):
#   Configure one or more sources to seed the PRNG of the
#   SSL library. The seed data should be of good random quality.
#   WARNING! On some platforms /dev/random blocks if not enough entropy
#   is available. This means you then cannot use the /dev/random device
#   because it would lead to very long connection times (as long as
#   it requires to make more entropy available). But usually those
#   platforms additionally provide a /dev/urandom device which doesn't
#   block. So, if available, use this one instead. Read the mod_ssl User
#   Manual for more details.
SSLRandomSeed startup file:/dev/urandom  256
SSLRandomSeed connect builtin
#SSLRandomSeed startup file:/dev/random  512
#SSLRandomSeed connect file:/dev/random  512
#SSLRandomSeed connect file:/dev/urandom 512

#
# Use "SSLCryptoDevice" to enable any supported hardware
# accelerators. Use "openssl engine -v" to list supported
# engine names.  NOTE: If you enable an accelerator and the
# server does not start, consult the error logs and ensure
# your accelerator is functioning properly.
#
SSLCryptoDevice builtin
#SSLCryptoDevice ubsec

##
## SSL Virtual Host Context
##

<VirtualHost _default_:443>

# General setup for the virtual host, inherited from global configuration
#DocumentRoot "/var/www/html"
#ServerName www.example.com:443

# Use separate log files for the SSL virtual host; note that LogLevel
# is not inherited from httpd.conf.
ErrorLog logs/ssl_error_log
TransferLog logs/ssl_access_log
LogLevel warn

#   SSL Engine Switch:
#   Enable/Disable SSL for this virtual host.
SSLEngine on

#   SSL Protocol support:
# List the enable protocol levels with which clients will be able to
# connect.  Disable SSLv2 access by default:
SSLProtocol all -SSLv2

#   SSL Cipher Suite:
#   List the ciphers that the client is permitted to negotiate.
#   See the mod_ssl documentation for a complete list.
SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5:!SEED:!IDEA

#   Speed-optimized SSL Cipher configuration:
#   If speed is your main concern (on busy HTTPS servers e.g.),
#   you might want to force clients to specific, performance
#   optimized ciphers. In this case, prepend those ciphers
#   to the SSLCipherSuite list, and enable SSLHonorCipherOrder.
#   Caveat: by giving precedence to RC4-SHA and AES128-SHA
#   (as in the example below), most connections will no longer
#   have perfect forward secrecy - if the server's key is
#   compromised, captures of past or future traffic must be
#   considered compromised, too.
#SSLCipherSuite RC4-SHA:AES128-SHA:HIGH:MEDIUM:!aNULL:!MD5
#SSLHonorCipherOrder on

#   Server Certificate:
# Point SSLCertificateFile at a PEM encoded certificate.  If
# the certificate is encrypted, then you will be prompted for a
# pass phrase.  Note that a kill -HUP will prompt again.  A new
# certificate can be generated using the genkey(1) command.
SSLCertificateFile /etc/pki/tls/certs/localhost.crt

#   Server Private Key:
#   If the key is not combined with the certificate, use this
#   directive to point at the key file.  Keep in mind that if
#   you've both a RSA and a DSA private key you can configure
#   both in parallel (to also allow the use of DSA ciphers, etc.)
SSLCertificateKeyFile /etc/pki/tls/private/localhost.key

#   Server Certificate Chain:
#   Point SSLCertificateChainFile at a file containing the
#   concatenation of PEM encoded CA certificates which form the
#   certificate chain for the server certificate. Alternatively
#   the referenced file can be the same as SSLCertificateFile
#   when the CA certificates are directly appended to the server
#   certificate for convinience.
#SSLCertificateChainFile /etc/pki/tls/certs/server-chain.crt

#   Certificate Authority (CA):
#   Set the CA certificate verification path where to find CA
#   certificates for client authentication or alternatively one
#   huge file containing all of them (file must be PEM encoded)
#SSLCACertificateFile /etc/pki/tls/certs/ca-bundle.crt

#   Client Authentication (Type):
#   Client certificate verification type and depth.  Types are
#   none, optional, require and optional_no_ca.  Depth is a
#   number which specifies how deeply to verify the certificate
#   issuer chain before deciding the certificate is not valid.
#SSLVerifyClient require
#SSLVerifyDepth  10

#   Access Control:
#   With SSLRequire you can do per-directory access control based
#   on arbitrary complex boolean expressions containing server
#   variable checks and other lookup directives.  The syntax is a
#   mixture between C and Perl.  See the mod_ssl documentation
#   for more details.
#<Location />
#SSLRequire (    %{SSL_CIPHER} !~ m/^(EXP|NULL)/ \
#            and %{SSL_CLIENT_S_DN_O} eq "Snake Oil, Ltd." \
#            and %{SSL_CLIENT_S_DN_OU} in {"Staff", "CA", "Dev"} \
#            and %{TIME_WDAY} >= 1 and %{TIME_WDAY} <= 5 \
#            and %{TIME_HOUR} >= 8 and %{TIME_HOUR} <= 20       ) \
#           or %{REMOTE_ADDR} =~ m/^192\.76\.162\.[0-9]+$/
#</Location>

#   SSL Engine Options:
#   Set various options for the SSL engine.
#   o FakeBasicAuth:
#     Translate the client X.509 into a Basic Authorisation.  This means that
#     the standard Auth/DBMAuth methods can be used for access control.  The
#     user name is the `one line' version of the client's X.509 certificate.
#     Note that no password is obtained from the user. Every entry in the user
#     file needs this password: `xxj31ZMTZzkVA'.
#   o ExportCertData:
#     This exports two additional environment variables: SSL_CLIENT_CERT and
#     SSL_SERVER_CERT. These contain the PEM-encoded certificates of the
#     server (always existing) and the client (only existing when client
#     authentication is used). This can be used to import the certificates
#     into CGI scripts.
#   o StdEnvVars:
#     This exports the standard SSL/TLS related `SSL_*' environment variables.
#     Per default this exportation is switched off for performance reasons,
#     because the extraction step is an expensive operation and is usually
#     useless for serving static content. So one usually enables the
#     exportation for CGI and SSI requests only.
#   o StrictRequire:
#     This denies access when "SSLRequireSSL" or "SSLRequire" applied even
#     under a "Satisfy any" situation, i.e. when it applies access is denied
#     and no other module can change it.
#   o OptRenegotiate:
#     This enables optimized SSL connection renegotiation handling when SSL
#     directives are used in per-directory context.
#SSLOptions +FakeBasicAuth +ExportCertData +StrictRequire
<Files ~ "\.(cgi|shtml|phtml|php3?)$">
    SSLOptions +StdEnvVars
</Files>
<Directory "/var/www/cgi-bin">
    SSLOptions +StdEnvVars
</Directory>

#   SSL Protocol Adjustments:
#   The safe and default but still SSL/TLS standard compliant shutdown
#   approach is that mod_ssl sends the close notify alert but doesn't wait for
#   the close notify alert from client. When you need a different shutdown
#   approach you can use one of the following variables:
#   o ssl-unclean-shutdown:
#     This forces an unclean shutdown when the connection is closed, i.e. no
#     SSL close notify alert is send or allowed to received.  This violates
#     the SSL/TLS standard but is needed for some brain-dead browsers. Use
#     this when you receive I/O errors because of the standard approach where
#     mod_ssl sends the close notify alert.
#   o ssl-accurate-shutdown:
#     This forces an accurate shutdown when the connection is closed, i.e. a
#     SSL close notify alert is send and mod_ssl waits for the close notify
#     alert of the client. This is 100% SSL/TLS standard compliant, but in
#     practice often causes hanging connections with brain-dead browsers. Use
#     this only for browsers where you know that their SSL implementation
#     works correctly.
#   Notice: Most problems of broken clients are also related to the HTTP
#   keep-alive facility, so you usually additionally want to disable
#   keep-alive for those clients, too. Use variable "nokeepalive" for this.
#   Similarly, one has to force some clients to use HTTP/1.0 to workaround
#   their broken HTTP/1.1 implementation. Use variables "downgrade-1.0" and
#   "force-response-1.0" for this.
BrowserMatch "MSIE [2-5]" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0

#   Per-Server Logging:
#   The home of a custom SSL log file. Use this when you want a
#   compact non-error SSL logfile on a virtual host basis.
CustomLog logs/ssl_request_log \
          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"

</VirtualHost>
```

**userdir.conf**


```
#
# UserDir: The name of the directory that is appended onto a user's home
# directory if a ~user request is received.
#
# The path to the end user account 'public_html' directory must be
# accessible to the webserver userid.  This usually means that ~userid
# must have permissions of 711, ~userid/public_html must have permissions
# of 755, and documents contained therein must be world-readable.
# Otherwise, the client will only receive a "403 Forbidden" message.
#
<IfModule mod_userdir.c>
    #
    # UserDir is disabled by default since it can confirm the presence
    # of a username on the system (depending on home directory
    # permissions).
    #
    UserDir disabled

    #
    # To enable requests to /~user/ to serve the user's public_html
    # directory, remove the "UserDir disabled" line above, and uncomment
    # the following line instead:
    #
    #UserDir public_html
</IfModule>

#
# Control access to UserDir directories.  The following is an example
# for a site where these directories are restricted to read-only.
#
<Directory "/home/*/public_html">
    AllowOverride FileInfo AuthConfig Limit Indexes
    Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
    Require method GET POST OPTIONS
</Directory>
```

**welcome.conf**:

```
#
# This configuration file enables the default "Welcome" page if there
# is no default index page present for the root URL.  To disable the
# Welcome page, comment out all the lines below.
#
# NOTE: if this file is removed, it will be restored on upgrades.
#
<LocationMatch "^/+$">
    Options -Indexes
    ErrorDocument 403 /.noindex.html
</LocationMatch>

<Directory /usr/share/httpd/noindex>
    AllowOverride None
    Require all granted
</Directory>

Alias /.noindex.html /usr/share/httpd/noindex/index.html
Alias /noindex/css/bootstrap.min.css /usr/share/httpd/noindex/css/bootstrap.min.css
Alias /noindex/css/open-sans.css /usr/share/httpd/noindex/css/open-sans.css
Alias /images/apache_pb.gif /usr/share/httpd/noindex/images/apache_pb.gif
Alias /images/poweredby.png /usr/share/httpd/noindex/images/poweredby.png
```

##PKI Support

If you would like to enable PKI then you can set the certs the same way as above.  If you would like to add Revocation list support you can add them via config maps.  Add each Certificate Refocation List (CRL) to the config map and have the config map mount to the directory location /etc/ssl/crl.  The config maps are mounted as root so when the proxy comes up it will generrate the proper has files for each CRL in a directory it can write to: /etc/httpd/crl. The following keys can be added to support PKI:

```
  SSLCertificateFile /etc/ssl/server-certs/server.pem
  SSLCertificateKeyFile /etc/ssl/server-certs/server.key
  SSLCACertificateFile /etc/ssl/server-certs/ca.crt
  SSLVerifyClient require
  SSLCARevocationPath /etc/httpd/crl/
  SSLCARevocationCheck chain
  SSLVerifyDepth 10
  SSLProxyEngine ON
  SSLProxyVerify none
  SSLProxyCheckPeerCN OFF
```

If you would like to extract out USER infromation from the PKI CERT then add:

```
RequestHeader set USERNAME "%{SSL_CLIENT_S_DN_CN}s"
RequestHeader set SSL_CLIENT_S_DN "%{SSL_CLIENT_S_DN}s"
RequestHeader set SSL_CLIENT_S_CN "%{SSL_CLIENT_S_DN_CN}s"
```
after the virtual host setting.  The variable is arbtrary and can be called whatever you like.

The **server.pem**, **server.key**, and the **ca.crt** are for the SSL PKI certificate and the added keys allow authenticating/verifying certs connecting through the proxy.  We use the **SSLCARevocationPath** for you may have multiple CRL definitions and you can not catenate them so the Path definition is used.

## Installation in Openshift

**Assumption:** The omar-web-proxy-app docker image is pushed into the OpenShift server's internal docker registry and available to the project.

### Persistent Volumes

OMAR Web Proxy does not require any persistent volumes.

### ConfigMaps

The OMAR Web Proxy uses ConfigMaps to inject Apache configuration items as deployment time. All of the above configuration files are mounted to the running service. Namely, a ConfigMap with all configuration files found in */etc/httpd/conf.d* and */etc/ssl/server-certs* should be created.

### Environment variables

No special environment variables are necessary for the web proxy.
