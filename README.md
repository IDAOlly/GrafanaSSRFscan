# Grafana SSRF Scan
This project was developed as a part of [Digital Security](https://github.com/DSecurity) internship in the department of web security audit ["Summer of Hack 2022"](https://dsec.ru/about/vacancies/#internship).

This simple python script acts as the proof-of-concept of the SSRF vulnerability found in **Grafana 9.0.2**. 

In case of Grafana, this SSRF is not exactly a vulnerability, but a feauture.  Because of the essential Grafana function to check availability of a host before asking for data, we can abuse it to check any host and any port we want.
## Mechanism
When administrator is trying to add new data source to get metrics from and visualize them after, he can set any data source URL and execute the availability test. By doing that, exactly 5 requests are sent to the Grafana API on the Grafana server.  Last 2 requests are transmitted from the server's identity to the URL that administrator has set. 

So, by specifying the data source URL and executing Save&Test function on the administrator panel, we induce the server-side application to make requests to any location. Judging by error message after the test, we can conclude that the certain port is open or closed.
## Usage
First, we need to get Grafana administrator credentials. Secondly, create two text files with IPs and ports. Then execute the following command in the directory with the script:
```
python scanNet.py -H http://grafana_server.com:3000 -U admin -P admin --ip-file ipList.txt --port-file portList.txt
```
- -H is the URL of Grafana server with default port 3000
- -U is administrator's login
- -P is administrator's password
- --ip-file is path to the file with IP-addresses (one address on one line)
- --port-file is path to the file with ports (one port on one line)

Using proxy is available with **-p** flag:
```
python scanNet.py -H http://grafana_server.com:3000 -U admin -P admin -p http://localhost:8080 --ip-file ipList.txt --port-file portList.txt
```
- -p is URL of the proxy server

The script is checking each port from the ports file on every IP address from the ip file.
In the result you can see If port is open or not.
