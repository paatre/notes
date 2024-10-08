# DNS - Domain Name System

## DNS records

### A record

The "A" stands for "address" and this is the most fundamental type of DNS record: it indicates the IP address of a given domain.

```sh
$ dig example.com A

;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		94	IN	A	216.58.209.174
```

### AAAA record

The same as the A record, but for IPv6 addresses.

```sh
$ dig google.com AAAA

;; QUESTION SECTION:
;google.com.			IN	AAAA

;; ANSWER SECTION:
google.com.		154	IN	AAAA	2a00:1450:4026:804::200e
```

### CNAME record

The "CNAME" stands for "canonical name" and this record is used to create an alias from one domain to another.

```sh
$ dig www.github.com CNAME

;; QUESTION SECTION:
;www.github.com.			IN	CNAME

;; ANSWER SECTION:
www.github.com.		3346	IN	CNAME	github.com.
```
### IN class

The "IN" stands for "Internet" and is the class of the record. It's the most common class used in DNS involving Internet hostnames, servers, or IP addresses.
