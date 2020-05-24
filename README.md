
Access Pod Online using podtnl
==========
[![wercker status](https://app.wercker.com/status/464ea2390bdb1592a8451cf187754be2/s/master "wercker status")](https://app.wercker.com/project/byKey/464ea2390bdb1592a8451cf187754be2)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Go Report Card](https://goreportcard.com/badge/github.com/narendranathreddythota/podtnl)](https://goreportcard.com/report/github.com/narendranathreddythota/podtnl)
[![Release](https://img.shields.io/badge/release-1.0-brightgreen.svg)](https://github.com/narendranathreddythota/podtnl/releases/tag/1.0)

<img align="center" width="1250" src="docs/images/tunnel.png">
<hr>
<img align="right" width="550" src="docs/images/recorder.gif">

 - Expose your pod to Online easily from any kubernetes clusters without creating a kubernetes service.

 - Clusters including minikube, kind, PKS, AKS, GKE, DK, etc

 - No need to worry about accessing your application during development, forgot about the following buzz words 
   [ingress, controller, loadbalancer, Public IP, etc]

**podtnl** uses two concepts: 
> - Port Forward
> - Tunnel

## Installation
**podtnl** is available in [homebrew](http://brew.sh/)
```shell
$ brew tap nnrthota/podtnl
$ brew install podtnl
```
**Build from Source**
```shell
$ go build 
$ cp podtnl /var/local/bin
```

## Usage:
```shell
$ podtnl -provider ngrok -podname couchdb0-64d95cccc5-5phqz -podport 5984

Expected Output:
[INFO] ...Tunnel provider ngrok
[INFO] NGROK is Ready
[INFO] Username: QoElOkoMmFv45f8kNKOCSVzyJz9zfakb
[INFO] Password: jApdHSdVLdpXYCkVgYmzYzSD70j9tipdYvgWLhrZ1mdHGvcngdHiHpdvJfQjAins
Forwarding from 127.0.0.1:5984 -> 5984
Forwarding from [::1]:5984 -> 5984
[INFO] mytunnel is created and Live: -> https://fa8df289.ngrok.io
Handling connection for 5984
Handling connection for 5984
Handling connection for 5984
Handling connection for 5984

^C[WARN] Shutting down all open tunnels..
[DBUG] Closing tunnel in https://fa8df289.ngrok.io
2020/05/24 00:23:21 interrupt
```
```shell
➜  ~ podtnl --help
Usage of podtnl:
  -Authentication
    	Need to secure the exposed pod with Basic Auth? (default true)
  -namespace string
    	Namespace where pod is running.. (default "default")
  -podname string
    	Pod Name
  -podport int
    	Pod Port
  -provider string
    	Provides Tunnel provider (default "ngrok")
  -providerPath string
    	Tunnel provider Path (default "/usr/local/bin/ngrok")
```
## Note:
If the following works perfectly 

```shell
$ kubectl port-forward pod/couchdb0-64d95cccc5-5phqz 5984:5984
```
Then the following also works
```shell
$ podtnl -provider ngrok -providerPath /usr/local/bin/ngrok -podname couchdb0-64d95cccc5-5phqz -podport 5984 -auth=false
```
> ***Do not forgot to hit <<<< ^C [CTL + C] >>>> in order to close the open tunnels***

## Error
### Wrong Pod 
```shell
➜  ~ podtnl -provider ngrok -podname somedummypodnameorwrongname -podport 5984
[INFO] ...Tunnel provider ngrok
[FTAL] error upgrading connection: pods "somedummypodnameorwrongname" not found
```
### Wrong Provider Path 
```shell
➜  ~ podtnl -provider ngrok -providerPath /usr/local/bin/ng -podname $POD -podport 5984
[INFO] ...Tunnel provider ngrok
2020/05/24 00:39:59 fork/exec /usr/local/bin/ng: no such file or directory
```
### Socat Error
#### For wrong Port or unable to port forward situations
```shell
E0524 00:42:00.609685   24854 portforward.go:400] an error occurred forwarding 5989 -> 5989: error forwarding port 5989 to pod 2f82fec0fb08efb22d2efcf02beb02b89a73398dd82c9a3e82103346c3f074f3, uid : exit status 1: 2020/05/23 20:42:00 socat[30542] E connect(5, AF=2 127.0.0.1:5989, 16): Connection refused
```
Licensing
=========
**podtnl** is licensed under the Apache License, Version 2.0. See
[LICENSE](https://github.com/narendranathreddythota/podtnl/blob/master/LISCENSE) for the full
license text.