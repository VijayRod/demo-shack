## File create

Create and initialize file with below.

```
# File: bash_dns.sh
# File initialize: chmod +x bash_dns.sh
```

#### File script for DNS resolution failures

Add below script to file for DNS resolution failures. File can be run with `./bash_dns.sh` or `./bash_dns.sh > /tmp/bash_dns.out`.

```
# Variables: Replace fqdncluster and fqdnsql in below.
fqdncluster='myaksclust-myaksrg-111111-0b9c24c4.hcp.southeastasia.azmk8s.io'
fqdnsql='mysqldb.database.windows.net'
date -u '+%Y-%m-%dT%H:%M:%SZ'
echo 'hostname: '$(hostname)
nslookup $fqdncluster
nslookup $fqdnsql
nslookup bing.com
nslookup deb.debian.org
nslookup google.com
```

#### File script for DNS latency issues

Add below script to file for DNS latency issues. File can be run with `./bash_dns.sh 5` or `./bash_dns.sh 5 > /tmp/bash_dns.out`.

```
echo 'hostname: '$(hostname)
nslookup bing.com
nslookup google.com
loops=$(($1+0))
for ((i=1;i<=$loops;i++))
do
	dt=`date -u '+%Y-%m-%dT%H:%M:%SZ'`
	curl -A `date +"[mycurltime:%Y-%m-%dT%H:%M:%S.%3N]"` -X GET 'https://www.google.com' --silent --output /dev/null -w "$dt status=%{http_code} %{redirect_url} size=%{size_download} time_namelookup=%{time_namelookup} time_connect=%{time_connect} time_appconnect=%{time_appconnect} time_pretransfer=%{time_pretransfer} time_redirect=%{time_redirect} time_starttransfer=%{time_starttransfer} time=%{time_total} num_redirects=%{num_redirects} speed_download=%{speed_download} speed_upload=%{speed_upload} num_connects=%{num_connects} content-type="%{content_type}" "
	sleep 2
done
```

#### Reference

Reference script implementation is at https://harrytechnotes.blogspot.com/2019/12/aks-1-five-seconds-latency-when.html .

## Manual script

Ref: https://github.com/Azure/AKS/issues/632

```
# Run a busybox or ubuntu shell inside a kubernetes cluster using something like this:
kubectl run my-shell --rm -i --tty --image ubuntu -- bash

# Once that shell launches run something like this until delay is observed:
apt update; apt install -y curl
for i in `seq 1 50`; do time curl -s google.com > /dev/null; done
```
