[![Build Status](https://travis-ci.com/netrixone/udig.svg?branch=master)](https://travis-ci.com/netrixone/udig)
[![Go Report Card](https://goreportcard.com/badge/github.com/netrixone/udig)](https://goreportcard.com/report/github.com/netrixone/udig)
[![Go Doc](https://godoc.org/github.com/netrixone/udig?status.svg)](https://godoc.org/github.com/netrixone/udig)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fnetrixone%2Fudig.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fnetrixone%2Fudig?ref=badge_shield)

# ÜberDig - dig on steroids

**Simple GoLang tool for domain recon.**

The purpose of this tool is to provide fast overview of a target domain setup. Several active scanning techniques
are employed for this purpose like DNS ping-pong, TLS certificate scraping or WHOIS banner parsing. Some tools on the other
hand are not - intentionally (e.g. nmap, brute-force, search engines etc.). This is not a full-blown DNS enumerator, 
but rather something more unobtrusive and fast which can be deployed in long-term experiments with lots of targets.

_**DISCLAIMER:** This tool is still under heavy development and the API might change without any considerations!_

Feature set:

- [x] Resolves a given domain to all DNS records of interest
- [x] Resolves a given domain to a set of WHOIS contacts (selected properties only)
- [x] Resolves a given domain to a TLS certificate chain
- [x] Supports automatic NS discovery with custom override
- [x] Dissects domains from resolutions and resolves them recursively
- [x] CLI application outputs all resolutions encoded as JSON strings
- [x] Supports multiple domains on the input
- [ ] Resolves IPs and domains found in SPF record
- [ ] Resolves domains in CSP header
- [ ] Supports a web-crawler to enumerate sub-domains
- [ ] Supports parallel resolution of multiple domains at the same time
- [x] Colorized CLI output
- [ ] Clean CLI output (e.g. RAW values, replace numeric constants with more meaningful strings)

## Download as dependency

`go get github.com/netrixone/udig`

## CLI app

### Build

`make`

### Usage

```bash
udig [-h|--help] [-v|--version] [-V|--verbose] [-d|--domain "<value>"
            [-d|--domain "<value>" ...]]

            ÜberDig - dig on steroids v1.0 by stuchl4n3k

Arguments:

  -h  --help     Print help information
  -v  --version  Print version and exit
  -V  --verbose  Be more verbose
  -d  --domain   Domain to resolve
```

### Example

```bash

$ udig -d example.com -V
 _   _ ____ ___ ____
(_) (_|  _ |_ _/ ___|
| | | | | | | | |  _
| |_| | |_| | | |_| |
 \__,_|____|___\____| v1.0

[~] DNS: Using NS a.iana-servers.net:53 for domain example.com.
[!] DNS: IXFR example.com -> NOTAUTH
[!] DNS: AXFR example.com -> NOTIMP
[~] WHOIS: Domain whois.iana.org is not related to example.com -> skipping.
[~] WHOIS: Domain res-dom.iana.org is not related to example.com -> skipping.
[~] TLS: Domain crl3.digicert.com is not related to example.com -> skipping.
[~] TLS: Domain crl4.digicert.com is not related to example.com -> skipping.
[~] TLS: Discovered a related domain example.org via example.com.
[~] TLS: Discovered a related domain example.edu via example.com.
[~] TLS: Discovered a related domain example.net via example.com.
[~] TLS: Domain digicert.com is not related to example.com -> skipping.
[~] DNS: Domain noc.dns.icann.org is not related to example.com -> skipping.
[~] DNS: Using NS a.iana-servers.net:53 for domain example.org.
[!] DNS: IXFR example.org -> NOTAUTH
[!] DNS: AXFR example.org -> NOTIMP
[~] DNS: Using NS b.iana-servers.net:53 for domain example.edu.
[!] DNS: IXFR example.edu -> NOTIMP
[!] DNS: AXFR example.edu -> NOTIMP
[~] DNS: Using NS a.iana-servers.net:53 for domain example.net.
[!] DNS: IXFR example.net -> NOTAUTH
[!] DNS: AXFR example.net -> NOTIMP
[+] WHOIS: example.com -> {"creation date":"1995-08-14t04:00:00z","registrar":"reserved-internet assigned numbers authority","registrar iana id":"376","registrar url":"http://res-dom.iana.org","registrar whois server":"whois.iana.org","registry domain id":"2336799_domain_com-vrsn","updated date":"2018-08-14t07:14:12z"}
[+] TLS: example.com -> {"SignatureAlgorithm":4,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":21020869104500376438182461249190639870,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":null,"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert SHA2 Secure Server CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,3],"Value":"DigiCert SHA2 Secure Server CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["Internet Corporation for Assigned Names and Numbers"],"OrganizationalUnit":["Technology"],"Locality":["Los Angeles"],"Province":["California"],"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"www.example.org","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,8],"Value":"California"},{"Type":[2,5,4,7],"Value":"Los Angeles"},{"Type":[2,5,4,10],"Value":"Internet Corporation for Assigned Names and Numbers"},{"Type":[2,5,4,11],"Value":"Technology"},{"Type":[2,5,4,3],"Value":"www.example.org"}],"ExtraNames":null},"NotBefore":"2018-11-28T00:00:00Z","NotAfter":"2020-12-02T12:00:00Z","KeyUsage":5,"Extensions":[{"Id":[2,5,29,35],"Critical":false},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,17],"Critical":false},{"Id":[2,5,29,15],"Critical":true},{"Id":[2,5,29,37],"Critical":false},{"Id":[2,5,29,31],"Critical":false},{"Id":[2,5,29,32],"Critical":false},{"Id":[1,3,6,1,5,5,7,1,1],"Critical":false},{"Id":[2,5,29,19],"Critical":true},{"Id":[1,3,6,1,4,1,11129,2,4,2],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":[1,2],"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":false,"MaxPathLen":-1,"MaxPathLenZero":false,"SubjectKeyId":"ZphiAuAJkafZ4zb7dsawv6Ftp74=","AuthorityKeyId":"D4BhHIIxYdUvKOeNRji0LOHG2eI=","OCSPServer":["http://ocsp.digicert.com"],"IssuingCertificateURL":["http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt"],"DNSNames":["www.example.org","example.com","example.edu","example.net","example.org","www.example.com","www.example.edu","www.example.net"],"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":["http://crl3.digicert.com/ssca-sha2-g6.crl","http://crl4.digicert.com/ssca-sha2-g6.crl"],"PolicyIdentifiers":[[2,16,840,1,114412,1,1],[2,23,140,1,2,2]]}
[+] TLS: example.com -> {"SignatureAlgorithm":4,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":2646203786665923649276728595390119057,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":null,"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert SHA2 Secure Server CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,3],"Value":"DigiCert SHA2 Secure Server CA"}],"ExtraNames":null},"NotBefore":"2013-03-08T12:00:00Z","NotAfter":"2023-03-08T12:00:00Z","KeyUsage":97,"Extensions":[{"Id":[2,5,29,19],"Critical":true},{"Id":[2,5,29,15],"Critical":true},{"Id":[1,3,6,1,5,5,7,1,1],"Critical":false},{"Id":[2,5,29,31],"Critical":false},{"Id":[2,5,29,32],"Critical":false},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,35],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":null,"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":true,"MaxPathLen":0,"MaxPathLenZero":true,"SubjectKeyId":"D4BhHIIxYdUvKOeNRji0LOHG2eI=","AuthorityKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","OCSPServer":["http://ocsp.digicert.com"],"IssuingCertificateURL":null,"DNSNames":null,"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":["http://crl3.digicert.com/DigiCertGlobalRootCA.crl","http://crl4.digicert.com/DigiCertGlobalRootCA.crl"],"PolicyIdentifiers":[[2,5,29,32,0]]}
[+] TLS: example.com -> {"SignatureAlgorithm":3,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":10944719598952040374951832963794454346,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"NotBefore":"2006-11-10T00:00:00Z","NotAfter":"2031-11-10T00:00:00Z","KeyUsage":97,"Extensions":[{"Id":[2,5,29,15],"Critical":true},{"Id":[2,5,29,19],"Critical":true},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,35],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":null,"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":true,"MaxPathLen":-1,"MaxPathLenZero":false,"SubjectKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","AuthorityKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","OCSPServer":null,"IssuingCertificateURL":null,"DNSNames":null,"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":null,"PolicyIdentifiers":null}
[+] DNS: example.com -> {"QueryType":1,"Record":{"Hdr":{"Name":"example.com.","Rrtype":1,"Class":1,"Ttl":86400,"Rdlength":4},"A":"93.184.216.34"}}
[+] DNS: example.com -> {"QueryType":6,"Record":{"Hdr":{"Name":"example.com.","Rrtype":6,"Class":1,"Ttl":3600,"Rdlength":45},"Ns":"sns.dns.icann.org.","Mbox":"noc.dns.icann.org.","Serial":2019041044,"Refresh":7200,"Retry":3600,"Expire":1209600,"Minttl":3600}}
[+] DNS: example.com -> {"QueryType":16,"Record":{"Hdr":{"Name":"example.com.","Rrtype":16,"Class":1,"Ttl":86400,"Rdlength":12},"Txt":["v=spf1 -all"]}}
[+] DNS: example.com -> {"QueryType":28,"Record":{"Hdr":{"Name":"example.com.","Rrtype":28,"Class":1,"Ttl":86400,"Rdlength":16},"AAAA":"2606:2800:220:1:248:1893:25c8:1946"}}
[+] DNS: example.com -> {"QueryType":47,"Record":{"Hdr":{"Name":"example.com.","Rrtype":47,"Class":1,"Ttl":3600,"Rdlength":26},"NextDomain":"www.example.com.","TypeBitMap":[1,2,6,16,28,46,47,48]}}
[+] DNS: example.com -> {"QueryType":255,"Record":{"Hdr":{"Name":"example.com.","Rrtype":6,"Class":1,"Ttl":3600,"Rdlength":45},"Ns":"sns.dns.icann.org.","Mbox":"noc.dns.icann.org.","Serial":2019041044,"Refresh":7200,"Retry":3600,"Expire":1209600,"Minttl":3600}}
[+] TLS: example.org -> {"SignatureAlgorithm":4,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":21020869104500376438182461249190639870,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":null,"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert SHA2 Secure Server CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,3],"Value":"DigiCert SHA2 Secure Server CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["Internet Corporation for Assigned Names and Numbers"],"OrganizationalUnit":["Technology"],"Locality":["Los Angeles"],"Province":["California"],"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"www.example.org","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,8],"Value":"California"},{"Type":[2,5,4,7],"Value":"Los Angeles"},{"Type":[2,5,4,10],"Value":"Internet Corporation for Assigned Names and Numbers"},{"Type":[2,5,4,11],"Value":"Technology"},{"Type":[2,5,4,3],"Value":"www.example.org"}],"ExtraNames":null},"NotBefore":"2018-11-28T00:00:00Z","NotAfter":"2020-12-02T12:00:00Z","KeyUsage":5,"Extensions":[{"Id":[2,5,29,35],"Critical":false},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,17],"Critical":false},{"Id":[2,5,29,15],"Critical":true},{"Id":[2,5,29,37],"Critical":false},{"Id":[2,5,29,31],"Critical":false},{"Id":[2,5,29,32],"Critical":false},{"Id":[1,3,6,1,5,5,7,1,1],"Critical":false},{"Id":[2,5,29,19],"Critical":true},{"Id":[1,3,6,1,4,1,11129,2,4,2],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":[1,2],"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":false,"MaxPathLen":-1,"MaxPathLenZero":false,"SubjectKeyId":"ZphiAuAJkafZ4zb7dsawv6Ftp74=","AuthorityKeyId":"D4BhHIIxYdUvKOeNRji0LOHG2eI=","OCSPServer":["http://ocsp.digicert.com"],"IssuingCertificateURL":["http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt"],"DNSNames":["www.example.org","example.com","example.edu","example.net","example.org","www.example.com","www.example.edu","www.example.net"],"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":["http://crl3.digicert.com/ssca-sha2-g6.crl","http://crl4.digicert.com/ssca-sha2-g6.crl"],"PolicyIdentifiers":[[2,16,840,1,114412,1,1],[2,23,140,1,2,2]]}
[+] TLS: example.org -> {"SignatureAlgorithm":4,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":2646203786665923649276728595390119057,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":null,"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert SHA2 Secure Server CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,3],"Value":"DigiCert SHA2 Secure Server CA"}],"ExtraNames":null},"NotBefore":"2013-03-08T12:00:00Z","NotAfter":"2023-03-08T12:00:00Z","KeyUsage":97,"Extensions":[{"Id":[2,5,29,19],"Critical":true},{"Id":[2,5,29,15],"Critical":true},{"Id":[1,3,6,1,5,5,7,1,1],"Critical":false},{"Id":[2,5,29,31],"Critical":false},{"Id":[2,5,29,32],"Critical":false},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,35],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":null,"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":true,"MaxPathLen":0,"MaxPathLenZero":true,"SubjectKeyId":"D4BhHIIxYdUvKOeNRji0LOHG2eI=","AuthorityKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","OCSPServer":["http://ocsp.digicert.com"],"IssuingCertificateURL":null,"DNSNames":null,"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":["http://crl3.digicert.com/DigiCertGlobalRootCA.crl","http://crl4.digicert.com/DigiCertGlobalRootCA.crl"],"PolicyIdentifiers":[[2,5,29,32,0]]}
[+] TLS: example.org -> {"SignatureAlgorithm":3,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":10944719598952040374951832963794454346,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"NotBefore":"2006-11-10T00:00:00Z","NotAfter":"2031-11-10T00:00:00Z","KeyUsage":97,"Extensions":[{"Id":[2,5,29,15],"Critical":true},{"Id":[2,5,29,19],"Critical":true},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,35],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":null,"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":true,"MaxPathLen":-1,"MaxPathLenZero":false,"SubjectKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","AuthorityKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","OCSPServer":null,"IssuingCertificateURL":null,"DNSNames":null,"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":null,"PolicyIdentifiers":null}
[+] WHOIS: example.org -> {"creation date":"1995-08-31t04:00:00z","registrant country":"us","registrant organization":"icann","registrant state/province":"ca","registrar":"icann","registrar iana id":"376","registry domain id":"d2328855-lror","updated date":"2015-08-19t20:25:53z"}
[+] DNS: example.org -> {"QueryType":1,"Record":{"Hdr":{"Name":"example.org.","Rrtype":1,"Class":1,"Ttl":86400,"Rdlength":4},"A":"93.184.216.34"}}
[+] DNS: example.org -> {"QueryType":6,"Record":{"Hdr":{"Name":"example.org.","Rrtype":6,"Class":1,"Ttl":3600,"Rdlength":42},"Ns":"sns.dns.icann.org.","Mbox":"noc.dns.icann.org.","Serial":2018112925,"Refresh":7200,"Retry":3600,"Expire":1209600,"Minttl":3600}}
[+] DNS: example.org -> {"QueryType":16,"Record":{"Hdr":{"Name":"example.org.","Rrtype":16,"Class":1,"Ttl":86400,"Rdlength":12},"Txt":["v=spf1 -all"]}}
[+] DNS: example.org -> {"QueryType":16,"Record":{"Hdr":{"Name":"example.org.","Rrtype":16,"Class":1,"Ttl":86400,"Rdlength":33},"Txt":["2b3dee88837848bbae05e3532f427b10"]}}
[+] DNS: example.org -> {"QueryType":28,"Record":{"Hdr":{"Name":"example.org.","Rrtype":28,"Class":1,"Ttl":86400,"Rdlength":16},"AAAA":"2606:2800:220:1:248:1893:25c8:1946"}}
[+] DNS: example.org -> {"QueryType":47,"Record":{"Hdr":{"Name":"example.org.","Rrtype":47,"Class":1,"Ttl":3600,"Rdlength":26},"NextDomain":"www.example.org.","TypeBitMap":[1,2,6,16,28,46,47,48]}}
[+] DNS: example.org -> {"QueryType":255,"Record":{"Hdr":{"Name":"example.org.","Rrtype":6,"Class":1,"Ttl":3600,"Rdlength":42},"Ns":"sns.dns.icann.org.","Mbox":"noc.dns.icann.org.","Serial":2018112925,"Refresh":7200,"Retry":3600,"Expire":1209600,"Minttl":3600}}
[+] TLS: example.edu -> {"SignatureAlgorithm":4,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":21020869104500376438182461249190639870,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":null,"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert SHA2 Secure Server CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,3],"Value":"DigiCert SHA2 Secure Server CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["Internet Corporation for Assigned Names and Numbers"],"OrganizationalUnit":["Technology"],"Locality":["Los Angeles"],"Province":["California"],"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"www.example.org","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,8],"Value":"California"},{"Type":[2,5,4,7],"Value":"Los Angeles"},{"Type":[2,5,4,10],"Value":"Internet Corporation for Assigned Names and Numbers"},{"Type":[2,5,4,11],"Value":"Technology"},{"Type":[2,5,4,3],"Value":"www.example.org"}],"ExtraNames":null},"NotBefore":"2018-11-28T00:00:00Z","NotAfter":"2020-12-02T12:00:00Z","KeyUsage":5,"Extensions":[{"Id":[2,5,29,35],"Critical":false},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,17],"Critical":false},{"Id":[2,5,29,15],"Critical":true},{"Id":[2,5,29,37],"Critical":false},{"Id":[2,5,29,31],"Critical":false},{"Id":[2,5,29,32],"Critical":false},{"Id":[1,3,6,1,5,5,7,1,1],"Critical":false},{"Id":[2,5,29,19],"Critical":true},{"Id":[1,3,6,1,4,1,11129,2,4,2],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":[1,2],"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":false,"MaxPathLen":-1,"MaxPathLenZero":false,"SubjectKeyId":"ZphiAuAJkafZ4zb7dsawv6Ftp74=","AuthorityKeyId":"D4BhHIIxYdUvKOeNRji0LOHG2eI=","OCSPServer":["http://ocsp.digicert.com"],"IssuingCertificateURL":["http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt"],"DNSNames":["www.example.org","example.com","example.edu","example.net","example.org","www.example.com","www.example.edu","www.example.net"],"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":["http://crl3.digicert.com/ssca-sha2-g6.crl","http://crl4.digicert.com/ssca-sha2-g6.crl"],"PolicyIdentifiers":[[2,16,840,1,114412,1,1],[2,23,140,1,2,2]]}
[+] TLS: example.edu -> {"SignatureAlgorithm":4,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":2646203786665923649276728595390119057,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":null,"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert SHA2 Secure Server CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,3],"Value":"DigiCert SHA2 Secure Server CA"}],"ExtraNames":null},"NotBefore":"2013-03-08T12:00:00Z","NotAfter":"2023-03-08T12:00:00Z","KeyUsage":97,"Extensions":[{"Id":[2,5,29,19],"Critical":true},{"Id":[2,5,29,15],"Critical":true},{"Id":[1,3,6,1,5,5,7,1,1],"Critical":false},{"Id":[2,5,29,31],"Critical":false},{"Id":[2,5,29,32],"Critical":false},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,35],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":null,"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":true,"MaxPathLen":0,"MaxPathLenZero":true,"SubjectKeyId":"D4BhHIIxYdUvKOeNRji0LOHG2eI=","AuthorityKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","OCSPServer":["http://ocsp.digicert.com"],"IssuingCertificateURL":null,"DNSNames":null,"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":["http://crl3.digicert.com/DigiCertGlobalRootCA.crl","http://crl4.digicert.com/DigiCertGlobalRootCA.crl"],"PolicyIdentifiers":[[2,5,29,32,0]]}
[+] TLS: example.edu -> {"SignatureAlgorithm":3,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":10944719598952040374951832963794454346,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"NotBefore":"2006-11-10T00:00:00Z","NotAfter":"2031-11-10T00:00:00Z","KeyUsage":97,"Extensions":[{"Id":[2,5,29,15],"Critical":true},{"Id":[2,5,29,19],"Critical":true},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,35],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":null,"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":true,"MaxPathLen":-1,"MaxPathLenZero":false,"SubjectKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","AuthorityKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","OCSPServer":null,"IssuingCertificateURL":null,"DNSNames":null,"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":null,"PolicyIdentifiers":null}
[+] DNS: example.edu -> {"QueryType":1,"Record":{"Hdr":{"Name":"example.edu.","Rrtype":1,"Class":1,"Ttl":86400,"Rdlength":4},"A":"93.184.216.34"}}
[+] DNS: example.edu -> {"QueryType":6,"Record":{"Hdr":{"Name":"example.edu.","Rrtype":6,"Class":1,"Ttl":3600,"Rdlength":45},"Ns":"sns.dns.icann.org.","Mbox":"noc.dns.icann.org.","Serial":2018112813,"Refresh":7200,"Retry":3600,"Expire":1209600,"Minttl":3600}}
[+] DNS: example.edu -> {"QueryType":16,"Record":{"Hdr":{"Name":"example.edu.","Rrtype":16,"Class":1,"Ttl":86400,"Rdlength":12},"Txt":["v=spf1 -all"]}}
[+] DNS: example.edu -> {"QueryType":28,"Record":{"Hdr":{"Name":"example.edu.","Rrtype":28,"Class":1,"Ttl":86400,"Rdlength":16},"AAAA":"2606:2800:220:1:248:1893:25c8:1946"}}
[+] DNS: example.edu -> {"QueryType":47,"Record":{"Hdr":{"Name":"example.edu.","Rrtype":47,"Class":1,"Ttl":3600,"Rdlength":26},"NextDomain":"www.example.edu.","TypeBitMap":[1,2,6,16,28,46,47,48]}}
[+] WHOIS: example.net -> {"creation date":"1995-08-31t04:00:00z","registrar":"reserved-internet assigned numbers authority","registrar iana id":"376","registrar url":"http://res-dom.iana.org","registrar whois server":"whois.iana.org","registry domain id":"2328120_domain_net-vrsn","updated date":"2018-08-31t07:04:10z"}
[+] TLS: example.net -> {"SignatureAlgorithm":4,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":21020869104500376438182461249190639870,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":null,"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert SHA2 Secure Server CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,3],"Value":"DigiCert SHA2 Secure Server CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["Internet Corporation for Assigned Names and Numbers"],"OrganizationalUnit":["Technology"],"Locality":["Los Angeles"],"Province":["California"],"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"www.example.org","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,8],"Value":"California"},{"Type":[2,5,4,7],"Value":"Los Angeles"},{"Type":[2,5,4,10],"Value":"Internet Corporation for Assigned Names and Numbers"},{"Type":[2,5,4,11],"Value":"Technology"},{"Type":[2,5,4,3],"Value":"www.example.org"}],"ExtraNames":null},"NotBefore":"2018-11-28T00:00:00Z","NotAfter":"2020-12-02T12:00:00Z","KeyUsage":5,"Extensions":[{"Id":[2,5,29,35],"Critical":false},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,17],"Critical":false},{"Id":[2,5,29,15],"Critical":true},{"Id":[2,5,29,37],"Critical":false},{"Id":[2,5,29,31],"Critical":false},{"Id":[2,5,29,32],"Critical":false},{"Id":[1,3,6,1,5,5,7,1,1],"Critical":false},{"Id":[2,5,29,19],"Critical":true},{"Id":[1,3,6,1,4,1,11129,2,4,2],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":[1,2],"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":false,"MaxPathLen":-1,"MaxPathLenZero":false,"SubjectKeyId":"ZphiAuAJkafZ4zb7dsawv6Ftp74=","AuthorityKeyId":"D4BhHIIxYdUvKOeNRji0LOHG2eI=","OCSPServer":["http://ocsp.digicert.com"],"IssuingCertificateURL":["http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt"],"DNSNames":["www.example.org","example.com","example.edu","example.net","example.org","www.example.com","www.example.edu","www.example.net"],"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":["http://crl3.digicert.com/ssca-sha2-g6.crl","http://crl4.digicert.com/ssca-sha2-g6.crl"],"PolicyIdentifiers":[[2,16,840,1,114412,1,1],[2,23,140,1,2,2]]}
[+] TLS: example.net -> {"SignatureAlgorithm":4,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":2646203786665923649276728595390119057,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":null,"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert SHA2 Secure Server CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,3],"Value":"DigiCert SHA2 Secure Server CA"}],"ExtraNames":null},"NotBefore":"2013-03-08T12:00:00Z","NotAfter":"2023-03-08T12:00:00Z","KeyUsage":97,"Extensions":[{"Id":[2,5,29,19],"Critical":true},{"Id":[2,5,29,15],"Critical":true},{"Id":[1,3,6,1,5,5,7,1,1],"Critical":false},{"Id":[2,5,29,31],"Critical":false},{"Id":[2,5,29,32],"Critical":false},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,35],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":null,"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":true,"MaxPathLen":0,"MaxPathLenZero":true,"SubjectKeyId":"D4BhHIIxYdUvKOeNRji0LOHG2eI=","AuthorityKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","OCSPServer":["http://ocsp.digicert.com"],"IssuingCertificateURL":null,"DNSNames":null,"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":["http://crl3.digicert.com/DigiCertGlobalRootCA.crl","http://crl4.digicert.com/DigiCertGlobalRootCA.crl"],"PolicyIdentifiers":[[2,5,29,32,0]]}
[+] TLS: example.net -> {"SignatureAlgorithm":3,"PublicKeyAlgorithm":1,"Version":3,"SerialNumber":10944719598952040374951832963794454346,"Issuer":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"Subject":{"Country":["US"],"Organization":["DigiCert Inc"],"OrganizationalUnit":["www.digicert.com"],"Locality":null,"Province":null,"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"DigiCert Global Root CA","Names":[{"Type":[2,5,4,6],"Value":"US"},{"Type":[2,5,4,10],"Value":"DigiCert Inc"},{"Type":[2,5,4,11],"Value":"www.digicert.com"},{"Type":[2,5,4,3],"Value":"DigiCert Global Root CA"}],"ExtraNames":null},"NotBefore":"2006-11-10T00:00:00Z","NotAfter":"2031-11-10T00:00:00Z","KeyUsage":97,"Extensions":[{"Id":[2,5,29,15],"Critical":true},{"Id":[2,5,29,19],"Critical":true},{"Id":[2,5,29,14],"Critical":false},{"Id":[2,5,29,35],"Critical":false}],"ExtraExtensions":null,"UnhandledCriticalExtensions":null,"ExtKeyUsage":null,"UnknownExtKeyUsage":null,"BasicConstraintsValid":true,"IsCA":true,"MaxPathLen":-1,"MaxPathLenZero":false,"SubjectKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","AuthorityKeyId":"A95QNVbRTLtm8KPiGxvDl7I90VU=","OCSPServer":null,"IssuingCertificateURL":null,"DNSNames":null,"EmailAddresses":null,"IPAddresses":null,"URIs":null,"PermittedDNSDomainsCritical":false,"PermittedDNSDomains":null,"ExcludedDNSDomains":null,"PermittedIPRanges":null,"ExcludedIPRanges":null,"PermittedEmailAddresses":null,"ExcludedEmailAddresses":null,"PermittedURIDomains":null,"ExcludedURIDomains":null,"CRLDistributionPoints":null,"PolicyIdentifiers":null}
[+] DNS: example.net -> {"QueryType":1,"Record":{"Hdr":{"Name":"example.net.","Rrtype":1,"Class":1,"Ttl":86400,"Rdlength":4},"A":"93.184.216.34"}}
[+] DNS: example.net -> {"QueryType":6,"Record":{"Hdr":{"Name":"example.net.","Rrtype":6,"Class":1,"Ttl":3600,"Rdlength":45},"Ns":"sns.dns.icann.org.","Mbox":"noc.dns.icann.org.","Serial":2019041033,"Refresh":7200,"Retry":3600,"Expire":1209600,"Minttl":3600}}
[+] DNS: example.net -> {"QueryType":16,"Record":{"Hdr":{"Name":"example.net.","Rrtype":16,"Class":1,"Ttl":86400,"Rdlength":12},"Txt":["v=spf1 -all"]}}
[+] DNS: example.net -> {"QueryType":28,"Record":{"Hdr":{"Name":"example.net.","Rrtype":28,"Class":1,"Ttl":86400,"Rdlength":16},"AAAA":"2606:2800:220:1:248:1893:25c8:1946"}}
[+] DNS: example.net -> {"QueryType":47,"Record":{"Hdr":{"Name":"example.net.","Rrtype":47,"Class":1,"Ttl":3600,"Rdlength":26},"NextDomain":"www.example.net.","TypeBitMap":[1,2,6,16,28,46,47,48]}}
[+] DNS: example.net -> {"QueryType":255,"Record":{"Hdr":{"Name":"example.net.","Rrtype":6,"Class":1,"Ttl":3600,"Rdlength":45},"Ns":"sns.dns.icann.org.","Mbox":"noc.dns.icann.org.","Serial":2019041033,"Refresh":7200,"Retry":3600,"Expire":1209600,"Minttl":3600}}
```

## Dependencies

* https://github.com/akamensky/argparse - Argparse for golang
* https://github.com/miekg/dns - DNS library in Go 
* https://github.com/domainr/whois - Whois client for Go

## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fnetrixone%2Fudig.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fnetrixone%2Fudig?ref=badge_large)