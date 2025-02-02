# Knock Subdomain Scan v5.0.0

Knockpy is a python3 tool designed to enumerate subdomains on a target domain through dictionary attack.

![knockpy5](https://user-images.githubusercontent.com/41558/111915750-1bad8f80-8a78-11eb-951a-d5da1adc2bdc.png)

### Very simply
```$ knockpy domain.com```

# Install

You need python3, pip3, git.

```$ git clone https://github.com/guelfoweb/knock.git```

- edit ```knockpy/config.json```
- add your [virustotal](https://virustotal.com/) ```API_KEY``` and save.

```
"api": {
		"virustotal": "YOUR VIRUSTOTAL API_KEY HERE"
	},
```

**Choose one of the three installation methods**

###### Install in the __global__ site-packages directory:

- as root

```# python setup.py install```

###### Install in the __user__ site-packages directory:

```$ python setup.py install --user```

###### Use virtualenv + pip

```virtualenv --python=python3 venv3```

```source venv3/bin/activate```

```pip install -r requirements.txt```

Are you looking for a [dockerized image of knockpy](https://github.com/guelfoweb/knock#knockpy-docker)?


# Knockpy -h

```
$ knockpy -h
usage: knockpy [-h] [-v] [--no-local] [--no-remote] [--no-http] [-w WORDLIST] [-o FOLDER] [-t SEC] domain

positional arguments:
  domain         target to scan

optional arguments:
  -h, --help     show this help message and exit
  -v, --version  show program's version number and exit
  --no-local     local wordlist ignore
  --no-remote    remote wordlist ignore
  --no-http      http requests ignore

  --no-http-code CODE [CODE ...]
                 http code list to ignore

  -w WORDLIST    wordlist file to import
  -o FOLDER      report folder to store json results
  -t SEC         timeout in seconds
```

# Usage

### Full scan
```$ knockpy domain.com```

- Attack type: **dns** + **http(s)** requests
- Knockpy uses internal file ```wordlist.txt```. If you want to use an external dictionary you can use the ```-w``` option and specify the path to your dictionary text file.
- Knockpy also tries to get subdomains from ```google```, ```duckduckgo```, and ```virustotal```. The results will be added to the general dictionary.
- It is highly recommended to use a [virustotal](https://virustotal.com/) ```API_KEY``` which you can get for free. The best results always come from ```virustotal```.
- But, if you only want to work with local word lists, without search engines queries, you can add ```--no-remote``` to bypass remote recon.
- If you want to ignore http(s) responses with specific code, you can use the ```--no-http-code``` option followed by the code list ```404 500 530```

### Fast scan
```$ knockpy domain.com --no-http```

- Attack type: **dns**
- DNS requests only, no http(s) requests will be made. This way the response will be much faster and you will get the IP address and the Subdomain.
- The subdomain will be cyan in color if it is an ```alias``` and in that case the real host name will also be provided.

### Set timeout
```$ knockpy domain.com -t 5```

- default timeout = ```3``` seconds.

### Show report
```$ knockpy domain.com_yyyy_mm_dd_hh_mm_ss.json```
- Show the report in the terminal.

### Output folder
```$ knockpy domain.com -o /path/to/new/folder```

- All scans are saved in the default folder ```knock_report``` that you can edit in the ```config.json``` file. 
- Alternatively, you can use the ```-o``` option to define the new folder path.

### Report
- At each scan the report will be automatically saved in ```json``` format inside the file with the name ```domain.com_yyyy_mm_dd_hh_mm_ss.json```.
- If you don't like autosave you can disable it from the ```config.json``` file by changing the value to ```"save": false```.
- To read the report in a human format you can do as described in [Show report](https://github.com/guelfoweb/knock#show-report).

Report example ```domain.com_yyyy_mm_dd_hh_mm_ss.json```:

```
{
    "sub-1.domain.com": {
        "domain": "host.domain.ext",
        "alias": ["sub-1.domain.com"],
        "ipaddr": [
            "123.123.123.123"
        ],
        "code": 200,
        "server": "Microsoft-IIS/8.5"
    },
    ...................................
               -- cut --
    ...................................
    "sub-n.domain.com"{
        "domain": "",
        "alias": [],
        "ipaddr": [
            "123.123.123.124"
        ],
        "code": 500,
        "server": "nginx/1.15.6 "
    },
    "_meta": {
        "name": "knockpy",
        "version": "5.0.0",
        "time_start": 1616353591.2510355,
        "time_end": 1616353930.6632543,
        "domain": "domain.com",
        "wordlist": 2120
    }
}
```

```_meta``` is a reserved key that contains the basic information of the scan.

### Knockpy docker

A dockerized image is hosted on [nocommentlab/knock](https://hub.docker.com/r/nocommentlab/knock) a project of [Antonio Blescia](https://github.com/nocommentlab).

You can use classic docker commands or run [kdocker](https://raw.githubusercontent.com/guelfoweb/knock/master/kdocker) script.

```./kdocker domain.com <arg1> <arg2> <argn>```

# License

Knockpy is currently under development by [@guelfoweb](https://twitter.com/guelfoweb) and it's released under the GPL 3 license.
