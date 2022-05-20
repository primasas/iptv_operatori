# Výdej online reklamy u IPTV operátorů

#### Popis

Požadavky na reklamní systém probíhají přes HTTPS protokol a typu volání GET bez použití proxy. Dotaz na reklamní systém musí probíhat v reálném čase podle předem definovaných značek, které nastavuje operátor a v případě nahrazování TV breaků se řídí dle FTV Prima API a timestampu. Při nahrazování je nutné počítat s posunem podle typu přenosu. Součásti dotazu již musí být také dopředu známá celková délka reklamního breaku, předávána v parametru duration, na základě které reklamní systém vrátí odpovídající sadu reklamních spotů. 


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

#### Formát odpovědi reklamního systému - VAST 4.2

Odpověď reklamního systému je VAST 4.2 s multi AdPods. Zpracování jednotlivých videospotu na straně přehrávače musí odpovídat minimálně tomuto standardu. 


------


## Seznam parametrů reklamního dotazu

| Base url |
| -------- |
| http(s)://a.iprima.cz/dserver/ |


| Význam | Parametr | Hodnota | Popis |
| ------ | ------ | ------ | ------ |
| Název operátora | operator | Nazevoperatora | predefinovaný, alfanumeric |
| Kanál | site | Nazevoperatora_KANAL | predefinovaný, složený z názvu operátora a názvu kanálu |
| Zařízení | section | smart_tv, mobile, web_desktop, web_mobile, mobile_tablet, web_tablet | určení zařízení |
| Typ reklamní pozice | area | preroll-1, midroll-7, midroll-1 | určení typu breaku / preroll-1 (60s) / midroll-7 (tv break duration) / midroll-1 (120s, 75s)|
| Reklamní typ | size | kombinace - spot,preroll  / spot,midroll | fixni hodnota |
| Délka breaku | duration | 60, 120, 75, dle délky tv breaku | fixní hodnota dle typu, při nahrazování simulcastu dle délky tv breaku |
| Validace na VAST 3 | format | validvast3 | fixní hodnota |
| Nastavení formátu | formatmt | application%2Fxml | fixní hodnota |
| Interní deklarace pro S2S | SUPERTAG | InstreamVideo | fixní hodnota |
| Doplňující data k obsahu | keyword | z API: name, Clip_id, Product_id | multivalue, alfanumeric, oddělovač čárka |
| Název pořadu | showname | nazev_poradu | z api Prima, ošetření formátu bez diakritiky, odstranění speciálních znaků, mezera, pomlčka je nahrazena podtržítkem, pouze mala písmena |
| Cachebuster | random | random hodnota | hash, 10ti místné číslo  |
| Označení breaku | viewid | random hodnota | hash, 10ti místné číslo , unikátní v rámci volání breaku, zamezení duplicity spotů |
| Device id  | mid | nazevoperatora_hashID | pokud má operátor vlastní určtení unikátního zařízení, složený z názvu operátora a zahashovaného id zařízení |
| Inzertní segmenty | seg1 až segN | - | hodnota bude doplněna po vzájemné konzultaci |
| Typ varianty | variant | varianta2, varianta3 | určení typu varianty, podle které se účtuje |
| Typ vysílání | broadcasting | livestream, startover, vosdalt3,ts47 | určení vysílacího modelu |
| Clip id | clipid | - | interní párovací číslo média - rE37563 - dostupné v Prima api |
| Product id | productid | - | unikátní výrobní číslo. Lomítka musí být nahrazena za pomlčku!  - dostupné v Prima api |
| Souhlas | consent | tcstring base 64 | operátor předá souhlas od uživatele dle TFC v2 - formát tcString |
| Aktivace GDPR | gdpr | 1 / 0 | fixní hodnota - V případě, kdy operátor není schopen předat souhlas musí být nastavena hodnota 0, aby byla zachována funkčnost liminace stejného spotu ve volání |



#### Seznam názvů kanálu pro parametr site


Duležité! - Kombinace názvů musí být předem definovány v reklamním systému. Nastavení použivaných názvů bude po dohodě s FTV Prima.

| Kanál | Kombinace názvu operátora a kanálu |
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


#### Zpracování videospotu dle standardu VAST 4.2

Zpracování reklamy na straně přehrávače musí být dle standardu iab VAST 4.2 - https://iabtechlab.com/wp-content/uploads/2019/06/VAST_4.2_final_june26.pdf


#### Video formát online reklamy
|  |  |
| ------ | ------ |
| Rozlišení | aspekt 16:9 (PAL Widescreen, anamorfní, bez čtvercovych pixelu) Max. bitrate 6 Mbps |
|  | 1280x720 @ 3000 kbps, 640x360 @ 750 kbps, 1024x576 @ 1500 kbps, 320x180 @ 450 kbps |
| Formát | MP4 (h.264), M3U8, WEBM |
| Délký spotů | 6s, 15s, 20s, 25s, 30s |
| Zvuk | AAC, 48 kHz - bez uměle zesíleného zvuku (cca 16,5 dB) Zvukový mix musí respektovat doporučení EBU R128, zvuková úroveň pořadu musí být normalizovánana -23 LUFS v integračním módu měření, maximální povolená hodnota modulace je -1 dBTP |


#### GDPR - předávání souhlasů (tcString)

- Souhlas platný dle legislativy, získá operátor na svých zařízeních.
- Seznam poskytovatelů CMP validovaných iAB - https://iabeurope.eu/cmp-list/ 
- Kontrolu validního formátu lze provést https://www.consentstringdecoder.com/

## Příklady volání pozic

### TV zařízení VOSDAL-T3
#### Preroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=smart_tv/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=vosdalt3/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

#### Midroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=smart_tv/area=midroll-1/size=spot,midroll/duration=120/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=vosdalt3/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`


### TV zařízení TS 4-7
#### Preroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=smart_tv/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=ts47/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

#### Midroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=smart_tv/area=midroll-1/size=spot,midroll/duration=75/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=ts47/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`


### Aplikace - Živé vysílání

#### Midroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=midroll-7/size=spot,midroll/duration=360/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=livestream/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

### Aplikace - Start-over

#### Preroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=startover/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

#### Midroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=midroll-7/size=spot,midroll/duration=360/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=startover/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

### Aplikace - VOSDAL T-3

#### Preroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=vosdalt3/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`
#### Midroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=midroll-1/size=spot,midroll/duration=120/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=vosdalt3/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

### Aplikace - TS 4-7

#### Preroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=ts47/clipid=clipid/productid=productid/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

#### Midroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=midroll-1/size=spot,midroll/duration=75/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=ts47/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

