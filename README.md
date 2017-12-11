# ScaleIT Registration Sidecar Overview

Sidecar Application Registration on ETCD Key/Value Store

This repo shows how to add a simple sidecar application to your Main-application to register at ETCD (Key/Value Store https://coreos.com/etcd/ ) with Name, Icon and a little Description.

This example application uses the following components:
* the ScaleIT Platform Essential App-Registry built with ETCD: https://github.com/ScaleIT-ORG/spe-app-registry-etcd
* the ScaleIT Registration Side Car: https://github.com/ScaleIT-ORG/spsc-app-registration

## Architecture

![Registration Sidecar Architecture Concept](https://github.com/ScaleIT-ORG/sidecar-registration-example/raw/master/Resources/Documentation/img/architecture.png)

# Installation

## Create ETCD Store 
	
	If you have not already installed and started the app-registry-etcd, follow the guide here:  https://github.com/ScaleIT-ORG/sppe-app-registry-etcd

## Clone this Repo

### Recursive

	This repo contains the sidecar repo as submodule. Clone this repo with --recursive to get all Submodules 
	e.g. git clone --recursive https://github.com/ScaleIT-ORG/sapp-sidecar-app-registration-example.git

### Standalone

	You could also clone this repo without the submodule and clone the sidecar repo manually in the `Platform Sidecars` folder (https://github.com/ScaleIT-ORG/spsc-app-registration.git)


##  Run

		- docker-compose build
		- docker-compose up

# How to use examples

## Check etcd healt status:
```curl -sb -H "Accept: application/json" "http://$ETCD_IP:49501/health"```

The Json answer should contain an attribute `"health": "true"`

## Set key/value pairs

`curl -L -X PUT http://localhost:49501/v2/keys/Example1/url -d value="my.url.com"`

## Get all entries

`http://localhost:49501/v2/keys?recursive=true`

## Get specific entry

`http://localhost:49501/v2/keys/Example1/url`

## Delete entry with all sub-values

`curl -L -X PUT "http://localhost:49501/v2/keys/Example1?recursive=true" -XDELETE`

## Get data with javascript

```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', "http://localhost:49501/v2/keys?recursive=true", true); 
xhr.send();
xhr.onreadystatechange = processRequest;

function processRequest(e) {
    if (xhr.readyState == 4 && xhr.status == 200) {
        var response = JSON.parse(xhr.responseText);
		console.log(response);
	}
}
```
