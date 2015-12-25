# kube2vulcand

Inspired by [kube2sky](https://github.com/kubernetes/kubernetes/blob/master/cluster/addons/dns/kube2sky/kube2sky.go)
kube2vulcand provides a a bridge between Kubernetes and [vulcand](http://vulcand.io). 

This will watch the kubernetes API for changes in Ingresses and then publish those changes to 
vulcand through etcd.
                        
For now, this is expected to be run in a pod alongside the etcd and vulcand containers.

## Usage

* `-etcd-mutation-timeout`: For how long the application will keep retrying etcd mutation (insertion or removal of a dns entry) before giving up and crashing.
* `-etcd-server`: The etcd server that is being used by skydns.
* `-kube-master-url`: URL of kubernetes master. Required if `--kubecfg-file` is not set.
* `-kubecfg-file`: Path to kubecfg file that contains the master URL and tokens to authenticate with the master.
* `-v`: Set logging level
* `-log_dir`: If non empty, write log files in this directory
* `-logtostderr`: Logs to stderr instead of files
                        
## Dev

* Setup the default kubernetes config file.

### Locally

* Start an etcd server
* `make run`

### docker-compose

This will start ectd, vulcand, and build the kube2vulcand image:

    make build
    export KUBE_CA_PATH=~/path/to/kube-ca # optional, based on your k8s config
    docker-compose up --force-recreate

## Deploy to kubernetes

This will create a daemonset with ectd, vulcand, and kube2vulcand, and a service to expose
the vulcand load balancer.

    kubectl create -f build/kube2vulcand.yaml