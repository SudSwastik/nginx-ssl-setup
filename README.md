# NGINX SSL SETUP


### Installation

> Create a VM in GCP

```
$ gcloud compute instances create <name> --project=<Project-id> --zone=us-central1-a --machine-type=e2-micro --network-interface=network-tier=PREMIUM,subnet=default --maintenance-policy=MIGRATE --service-account=<service-account> --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server,https-server --create-disk=auto-delete=yes,boot=yes,device-name=nginx-ssl,image=projects/debian-cloud/global/images/debian-10-buster-v20211209,mode=rw,size=10,type=projects/round-water-262008/zones/us-central1-a/diskTypes/pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any
```

> Login To VM and Install nginx

```
$ sudo apt update
$ sudo apt install nginx
```

> Create an A record with domain or subdomain pointing to ip of VM

> Generate SSL Certificate

```
$ brew install certbot
$ mkdir certbot
$ mkdir -p certbot/config
$ mkdir -p certbot/work
$ mkdir -p certbot/logs
$ certbot certonly --manual --preferred-challenges dns -d <domain || subdomain> --config-dir ./certbot/config --work-dir ./certbot/work/ --logs-dir ./certbot/logs/ 
```

> Add the TXT Record during certificate Generation to domain provider

> Inside the VM 

```
$ cd /etc/ssl
$ touch fullchain.pem
$ touch privkey.pem
```

> Copy the Whole fullchain.pem and privkey.pem file from local to a folder in VM.


> Modify the nginx website to use the ssl certificates by updating nginx.conf
```
$ sudo nginx -t
$ sudo systemctl reload nginx
```