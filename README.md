# Výdej online reklamy u IPTV operátorů

#### Popis

Požadavky na reklamní systém probíhají přes HTTPS protokol a typu volání GET bez použití proxy. Dotaz na reklamní systém musí probíhat v reálném čase podle předem definovaných značek, které nastavuje operátor a v případě nahrazování TV breaků se řídí dle FTV Prima API a timestampu. Při nahrazování je nutné počítat s posunem podle typu přenosu a je nutné nastraně operátora sesynchronizovat. Součásti dotazu již musí být také dopředu známá celková délka reklamního breaku, předávána v parametru duration, na základě které reklamní systém vrátí odpovídající sadu reklamních spoty.  


## Parametry dotazu a odpovědi reklamního serveru

#### Dotaz na server

| Název | Hodnota |
| ------ | ------ |
| Protokol | HTTPS |
| Typ | GET |
| Síť | Internet - bez proxy |


#### Odpověď serveru

| Název | Hodnota |
| ------ | ------ |
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

#### Odpověď s CORS

| Header | Value |
| ------ | ------ |
| Access-Control-Allow-Credentials | TRUE |
| Access-Control-Allow-Headers | X-Requested-With, origin, content-type, accept, accept-encoding, acceptlanguage, cache-control, dnt |
| Access-Control-Allow-Methods | GET, OPTIONS |
| Access-Control-Allow-Origin | < varies > |
| Access-Control-Max-Age | 600 |

#### Cookies

Each request sent to SAS® 360 Match is checked for a MID cookie. The MID cookie uniquely
identifies each visitor and establishes a visitor session that many ad-serving features rely on.
The MID cookie can be renamed.
The MID cookie supports HttpOnly and Secure flags

#### Chybové odpovědi

| Typ | Popis |
| ------ | ------ |
| 404 Invalid Job ID | When the specified job ID does not exist. |
| 408 Request Timeout | When the engine fails to respond in time to a request. |
| 429 Too Many Requests | When the server receives too many requests from a visitor. The response is sent until the condition subsides |
| 500 Internal Server Error | When there is an uncategorized error. |

## Formát odpovědireklamního systému - VAST 3.0

Odpověď reklamního systému je VAST 3.0 s multi AdPods. Zpracování jednotlivých videospotu na straně přehrávače musí odpovídat minimálně tomuto standardu. 

------

#### Seznam parametrů reklamního dotazu

| Význam | Parametr | Hodnota | Popis |
| ------ | ------ | ------ | ------ |
| Doména | - | a.iprima.cz | doména pro provolání dotazu |
| Typ Serveru | - | dserver | určení výdejového serveru |
| Kanál | site | Nazevoperatora_KANAL | predefinovaný, složený z názvu operátora a názvu kanálu |
| Zařízení | section | smart_tv,mobile,web_desktop, web_mobile, mobile_tablet,web_tablet | určení zařízení |
| Typ reklamní pozice | area | preroll-1, midroll-1, midroll-2 | určení typu breaku |
| Reklamní typ | size | kombinace - spot,preroll  / spot,midroll | fixni hodnota |
| Délka breaku | duration | 30, 360, dle délky tv breaku | fixní hodnota dle typu, při nahrazování simulcastu dle délky tv breaku |
| Validace na VAST 3 | format | validvast3 | fixní hodnota |
| Nastavení formátu | formatmt | application%2Fxml | fixní hodnota |
| Interní deklarace pro S2S | SUPERTAG | InstreamVideo | fixní hodnota |
| Doplňující data k obsahu | keyword | porad,typ | multivalue, alfanumeric, oddělovač čárka |
| Název pořadu | showname | nazev_poradu | z api Prima, ošetření formátu bez diakritiky, odstranění speciálních znaků, mezera, pomlčka je nahrazena podtržítkem |
| Označení breaku | viewid | random hodnota | hash, 10ti místné číslo , unikátní v rámci volání breaku, zamezení duplicity spotů |
| Cachebuster | random | random hodnota | hash, 10ti místné číslo  |
| Device id  | mid | nazevoperatora_hashID | pokud má operátor vlastní určtení unikátního zařízení, složený z názvu operátora a zahashovaného id zařízení |
| Inzertní segmenty | seg1 až segN | - | hodnota bude doplněna po vzájemné konzultaci |
| Název operátora | operator | Nazevoperatora | predefinovaný, alfanumeric |
| Typ varianty | variant | varianta2,varianta3 | určení typu varianty, podle které se účtuje |
| Typ vysílání | broadcasting | livestream,startover,vosdalt3,ts47,vosdalt7 | určení vysílacího modelu |
| Clip id | clipid | - | interní párovací číslo média - rE37563 |
| Product id | productid | - | unikátní výrobní číslo |
| Skippable | skip | 1 / 0 | z api Prima |
| Aktivace GDPR | gdpr | 1 | fixní hodnota |
| Souhlas | consent | tcstring base 64 | Operátor předá souhlas od uživatele dle TFC v2 - formát tcString |


#### Seznam názvu kanálu pro parametr site

Duležité! - Kombinace názvů musí být předem definovány v reklamním systému. Nastavení použivaných názvů po dohodě s FTV Prima

| Kanál | Kombinace názvu operátoru a kanálu |
| ------ | ------ |
| PRIMA | Nazevoperatora_PRIMA |
| COOL | Nazevoperatora_COOL |
| MAX | Nazevoperatora_MAX |
| ZOOM | Nazevoperatora_ZOOM |
| LOVE | Nazevoperatora_LOVE |
| KRIMI | Nazevoperatora_KRIMI |
| STAR | Nazevoperatora_STAR |
| CNN | Nazevoperatora_CNN |
| PRIMAPLUS | Nazevoperatora_PRIMAPLUS |
| SHOW | Nazevoperatora_SHOW |


#### Zpracování videospotu dle standardu VAST 3

Zpracování reklamy na straně přehrávače musí být dle standardu iab VAST 3.0 - https://www.iab.com/wp-content/uploads/2015/06/VASTv3_0.pdf


#### Příklad dotazu na reklamní systém

 https://a.iprima.cz/dserver/site=Nazevoperatora_KANAL/section=smart_tv/area=preroll-1/size=spot,preroll/duration=360/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=clipid,productid/showname=nazev_poradu/viewid=1234567890/random=1234567890/mid=nazevoperatora_hashID/seg1=/seg2==/operator=Nazevoperatora/variant=varianta2/broadcasting=vosdalt3/clipid=rE49300/productid=/skip=1/gdpr=1/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/