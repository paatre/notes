# Authentication

## Origins

Originated in 1961 with the MIT compatible time-sharing system (CTSS) as a way of solving the problem of governing who has access to what resources. The MIT computer at the time, the IBM 709, had a finite amount of resources and as a result, operators were asked to log in with credentials that would grant them a set of limits or resources on how much CPU time and memory that operator could use. Since then, this idea of governing access to resources is still the main reason why we have authentication.[1]

## Kerberos

Kerberos was a Massachusetts Institute of Technology (MIT) internal project, initially aiamed at protecting network ervies provided by Project Athena, a campus-wide distributed computing environment for educational use.[1][2] Kerberos was officially released to the public in the late 1980s and revised multiple times, with the current iteration (version 5) dating to 1993. Kerberos preceded SSL/TLS and it was introduced as a way to authenticate users over an insecure network where traffic was actively being intercepted. In doing so, it solved a common problem: cryptographic operations were expensive on CPUs back in the ’90s and early 2000s, making TLS for all connections unfeasible. Kerberos is a system that allows authentication without ever transmitting the password over the network thanks to the use of a series of ticket exchanges. Later on, Kerberos could install encryption layers similar to what TLS does today, transforming Kerberos into a protocol able to provide a root of trust.

## LDAP

Lightweight Directory Access Protocol, LDAP, was launched in 1997 as a successor to the directory access protocol which is part of the X500 standards. These were created by the International Telecom Union and included concepts we still use today, like x.509 certificates (which were initially intended to be the default directory authentication artefact).

LDAP is a binary protocol similar to SQL and HTTP that allows access to a directory database, which is comparable to a phone book. This phone book or directory contains a set of users nested in groups, and it allows a connected computer to query and organise that information. As a side effect of the way LDAP works, it is also possible to check user passwords that are associated with their accounts.

The data in these directories is not structured the same way as a table in SQL. Rather, it is much closer to JSON, with objects (or entries) containing a series of key-value pairs. The list of which key-value pairs can exist is defined by a construct called a "schema".

In addition, these entries can be nested and structured into a tree, allowing hierarchical oranisation searching capabilities, which enables many vital query and structural elements.

The most famous open-source LDAP implementation is OpenLDAP, which was released in 1998 by Kurt Zeilenga. It used most of the source code from the original LDAP project developed by the University of MIchigan in 1996.

### Example LDAP entry

```
dn: sn=ImpishIndri,ou=people,dc=ubuntu,dc=com
objectclass: person
sn: ImpishIndri
cn: Ubuntu ImpishIndri

dn: sn=FocalFossa,ou=people,dc=ubuntu,dc=com
objectclass: person
sn: FocalFossa
cn: Ubuntu FocalFossa
```

- [1]: History of Open Source Identity Management (part 1). https://ubuntu.com/blog/history-of-open-source-identity-management-part-1
- [2]: Project Athena. https://en.wikipedia.org/wiki/Project\_Athena
