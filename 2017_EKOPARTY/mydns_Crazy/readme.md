## Crazy (MYDNS)

> Try to connect to this crazy service, once you do it the answer will appear.
>
> http://crazy.mydns.webcam/

### Solution

When we first tried to access this service by a standard browser it looked like it was down. However since the task category suggests this might be DNS related problem we decided to check DNS records for this domain. 

It was a good decision becaus DNS query showed that *crazy.mydns.webcam* domain has quite a lot A records associated to it that look like some random IPs:

```bash
$ dig +trace crazy.mydns.webcam
...
crazy.mydns.webcam. 300 IN  A   70.215.170.136
crazy.mydns.webcam. 300 IN  A   56.242.86.52
crazy.mydns.webcam. 300 IN  A   74.47.147.237
crazy.mydns.webcam. 300 IN  A   77.29.224.136
crazy.mydns.webcam. 300 IN  A   85.192.7.60
crazy.mydns.webcam. 300 IN  A   70.18.170.196
crazy.mydns.webcam. 300 IN  A   41.113.55.170
crazy.mydns.webcam. 300 IN  A   85.70.195.186
crazy.mydns.webcam. 300 IN  A   67.198.229.203
crazy.mydns.webcam. 300 IN  A   74.101.141.79
crazy.mydns.webcam. 300 IN  A   76.37.174.171
crazy.mydns.webcam. 300 IN  A   87.177.248.219
crazy.mydns.webcam. 300 IN  A   79.62.241.244
...
```

Next we checked which of those IP addresses have 80/tcp port open and it turned out that only couple of them have:

```
42.200.213.149
45.34.187.253
46.231.248.170
52.85.145.177
54.154.180.107
54.235.129.98
66.160.176.236
67.198.229.203
69.64.95.17
82.127.136.30
85.192.7.60
```

We were pretty sure that not all of those IPs were managed by CTF organizers and consequently we didn't want to do too much "testing" on them. We decided to just query each of them for crazy.mydns.webcam vhost and search for the flag:

```bash
$ cat ips.txt | while read ip; do curl -s -D- -H 'Host: crazy.mydns.webcam' http://${ip}/ | grep 'EKO{'; done
<strong>EKO{m3ssing_w1th_round_r0b1n}</strong>
```

Bingo! The flag is **EKO{m3ssing_w1th_round_r0b1n}**
