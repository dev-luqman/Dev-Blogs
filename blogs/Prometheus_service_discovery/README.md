## Prometheus Service Discovery of EC2 Instance
It will be actually be fun if we can detect new instance created and have the exported data running and emitting data with out having to touch the prometheus server again. 
Yes we will configure prometheus server to auto collect data without to change any files again


### Prerequisites
1. Create prometheus server [ec2 instance](../CreateAWSInstance/README.md)
2. Configure Prometheus on server [Prometheus_configuration](../Prometheus_setUp/README.md)
3. Create a new instance to be monitor by following ` prerequisite 1 `

` Ensure that Security group of port 9100 is part of the ingress rule ` 

4.  Create IAM role programatic access and amin roles ` This step will discuss another time `
* we need to collect the ` secret key ` and ` accees key ` in this step


### Steps
#### 1.  SSH into your Prometheus Server

```
ssh -i prometheus.pem ubuntu@ec2-3-17-28.53.us-east-2.compute.amazonaws.com

```
Please replace the ` prometheus.pem ` with your key pair downloaded and also change ` ubuntu@ec2-3-17-28.53.us-east-2.compute.amazonaws.com ` to your dnsName


#### 2. Edit Prometheus Yml file once more
```
sudo vi /etc/prometheus/prometheus.yml

```

* Replace with the below code

```

global:
  scrape_interval: 1s
  evaluation_interval: 1s

scrape_configs:
  - job_name: 'node'
    ec2_sd_configs:
      - region: us-east-1 ` your region `
        access_key: PUT_THE_ACCESS_KEY_HERE
        secret_key: PUT_THE_SECRET_KEY_HERE
        port: 9100

```


#### 3. Restart Prometheus Service

```

sudo systemctl restart prometheus

```

### Final - check status again and we should be fine as previous 