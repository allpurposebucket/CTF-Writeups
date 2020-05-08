# **Proxy List**
## Points: 100
### **Description:** We need you to perform geolocation analysis on this list of IPs. We have attributed it to a malicious proxy network. Report back with the prevalent country of origin: [ips.txt](https://challenge.acictf.com/static/5c04f8032bb038c31547f8eb340c686d/ips.txt)
This challenge provides a text file of nearly 30000 IP addresses. We're told that the flag is the country with the most addresses in the list.
After creating a free account on ipinfo.io, I created the following python script. 

```python
from urllib import urlopen
from json import load
countryDict = {}
f = open("addresses.txt", 'r')
lines = f.readlines()
for line in lines:
    url = 'https://ipinfo.io/' + line + '/json' + '?token=myIPinfoToken
    res = urlopen(url)
    data = load(res)
    if data['country'] in countryDict:
        countryDict[data['country']] += 1
    else:
        country[data['country']] = 1
    max_value = max(countryDict.values())
    max_keys = [k for k, v in countryDict.items() if v == max_value]
    print(max_keys, max_value)   
```
## **Flag:** ACI{Brazil}
