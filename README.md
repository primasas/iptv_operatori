# Technické parametry výdej online reklamy u IPTV operátorů

Dotaz na reklamní systém musí probíhat v reálném čase podle předem definovaných značek pro přehrání v požadovaném čase. Součásti dotazu již musí být dopředu známá celková délka reklamního breaku předávána v parametru duration, na základě která reklamní systém vrátí odpovídající reklamní spoty.


## Technické parametry dotazu na reklamní systém:

##### Dotaz na server

| Protokol | HTTPS |
| Typ | GET |
| Síť | Internet - bez proxy |

##### Odpověď serveru

| HTTP | 200 OK |

| Header | Value |
| ------ | ------ |
| Cache-Control | no-cache, no-store, max-age=0, must-revalidate |
| Connection | keep-alive |
| Content-Encoding | gzip |
| Content-Length | < varies > |
| Content-Security-Policy | default-src 'self' |
| Content-Type | < varies > |
| Date | < varies > |
| Expires | -1 |
| Pragma | no-cache |
| Server | < varies > |
| Set-Cookie | < varies >, Secure, and HttpOnly flags optional. |
| Strict-Transport-Security | max-age=31536000; includeSubDomains |
| X-Content-Type-Options | nosniff |
| X-XSS-Protection | 1; mode=block |

##### Odpověď s CORS

| Header | Value |
| ------ | ------ |
| Access-Control-Allow-Credentials | TRUE |
| Access-Control-Allow-Headers | X-Requested-With, origin, content-type, accept, accept-encoding, acceptlanguage, cache-control, dnt |
| Access-Control-Allow-Methods | GET, OPTIONS |
| Access-Control-Allow-Origin | < varies > |
| Access-Control-Max-Age | 600 |

##### Cookies

Each request sent to SAS® 360 Match is checked for a MID cookie. The MID cookie uniquely
identifies each visitor and establishes a visitor session that many ad-serving features rely on.
The MID cookie can be renamed.
The MID cookie supports HttpOnly and Secure flags

##### Chybové odpovědi

| 404 Invalid Job ID | When the specified job ID does not exist. |
| 408 Request Timeout | When the engine fails to respond in time to a request. |
| 429 Too Many Requests | When the server receives too many requests from a visitor. The response is sent until the condition subsides |
| 500 Internal Server Error | When there is an uncategorized error. |


## Parametry dotazu na reklamní systém:

Odpověď reklamního systému je VAST 3 s multi AdPods.


##### Seznam parametrů

| Význam | Parametr | Hodnota | Popis |
| ------ | ------ |