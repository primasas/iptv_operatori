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

#### Formát odpovědi reklamního systému - VAST 3.0

Odpověď reklamního systému je VAST 3.0 s multi AdPods. Zpracování jednotlivých videospotu na straně přehrávače musí odpovídat minimálně tomuto standardu. 

------

## Seznam parametrů reklamního dotazu


| Význam | Parametr | Hodnota | Popis |
| ------ | ------ | ------ | ------ |
| Doména | - | a.iprima.cz | doména pro provolání dotazu |
| Typ serveru | - | dserver | určení výdejového serveru |
| Kanál | site | Nazevoperatora_KANAL | predefinovaný, složený z názvu operátora a názvu kanálu |
| Zařízení | section | smart_tv,mobile,web_desktop, web_mobile, mobile_tablet,web_tablet | určení zařízení |
| Typ reklamní pozice | area | preroll-1, midroll-1, midroll-2, midroll-3, midroll-4 | určení typu breaku |
| Reklamní typ | size | kombinace - spot,preroll  / spot,midroll | fixni hodnota |
| Délka breaku | duration | 30, 360, dle délky tv breaku | fixní hodnota dle typu, při nahrazování simulcastu dle délky tv breaku |
| Validace na VAST 3 | format | validvast3 | fixní hodnota |
| Nastavení formátu | formatmt | application%2Fxml | fixní hodnota |
| Interní deklarace pro S2S | SUPERTAG | InstreamVideo | fixní hodnota |
| Doplňující data k obsahu | keyword | z API: name, Clip_id | multivalue, alfanumeric, oddělovač čárka |
| Název pořadu | showname | nazev_poradu | z api Prima, ošetření formátu bez diakritiky, odstranění speciálních znaků, mezera, pomlčka je nahrazena podtržítkem, pouze mala písmena |
| Nastavení zvuku| mute | 1 / 0 | předání informace, jesti má přehrávač při startu reklamního blocku aktivní zvuk |
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


#### Zpracování videospotu dle standardu VAST 3

Zpracování reklamy na straně přehrávače musí být dle standardu iab VAST 3.0 - https://www.iab.com/wp-content/uploads/2015/06/VASTv3_0.pdf


#### Video formát online reklamy
|  |  |
| ------ | ------ |
| Rozlišení | aspekt 16:9 (PAL Widescreen, anamorfní, bez čtvercovych pixelu) Max. bitrate 6 Mbps |
|  | 1280x720 @ 3000 kbps, 640x360 @ 750 kbps, 1024x576 @ 1500 kbps, 320x180 @ 450 kbps |
| Formát | MP4 (h.264), M3U8, WEBM |
| Délký spotů | 6s, 15s, 20s, 25s, 30s |
| Zvuk | AAC, 48 kHz - bez uměle zesíleného zvuku (cca 16,5 dB) Zvukový mix musí respektovat doporučení EBU R128, zvuková úroveň pořadu musí být normalizovánana -23 LUFS v integračním módu měření, maximální povolená hodnota modulace je -1 dBTP |


------

## Rozdělení podle variant a zařízení


### Varianta 2

| Typ | TV | Mobile |
| ------ | ------ | ------ |
| Živé vysílání | - | max 12 minut do hodiny / 3.5 minuty nepřeskočitelné |
| Start-over | - | max 12 minut do hodiny / 3.5 minuty nepřeskočitelné |
| VOSDAL T3 | 1 minuta do hodiny / nepřeskočitelné |max 12 minut do hodiny / 3.5 minuty nepřeskočitelné |
| T4-T7 | max 12 minut do hodiny / 3.5 minuty nepřeskočitelné | max 12 minut do hodiny / 3.5 minuty nepřeskočitelné |

### Varianta 3

| Typ | TV | Mobile |
| ------ | ------ | ------ |
| Živé vysílání | - | max 12 minut do hodiny / nepřeskočitelné |
| Start-over | - | max 12 minut do hodiny / nepřeskočitelné |
| TS4-T7 | max 12 minut do hodiny / 3.5 minuty nepřeskočitelné | max 12 minut do hodiny / 3.5 minuty nepřeskočitelné |


## Rozdělení podle typu pozice a délky ( duration )


### Varianta 2

| Typ | TV | Mobile |
| ------ | ------ | ------ |
| Živé vysílání | - | 2x midroll, duration 360 / 3.5 minuty nepřeskočitelné |
| Start-over | - | 2x midroll, duration 360 / 3.5 minuty nepřeskočitelné |
| VOSDAL T3 | 1x preroll a 1x midroll, duration 30 / nepřeskočitelné | 2 až 4 midrolls s duration odpovídající 720 sekund do hodiny |
| T4-T7 |  2 až 4 midrolls s duration odpovídající 720 sekund do hodiny / 3.5 minuty nepřeskočitelné |  2 až 4 midrolls s duration odpovídající 720 sekund do hodiny  / 3.5 minuty nepřeskočitelné |

### Varianta 3

| Typ | TV | Mobile |
| ------ | ------ | ------ |
| Živé vysílání | - | 2x midroll, duration 360 / 3.5 minuty nepřeskočitelné |
| Start-over | - | 2x midroll, duration 360 / 3.5 minuty nepřeskočitelné |
| TS4-T7 |  2 až 4 midrolls s duration odpovídající 720 sekund do hodiny / 3.5 minuty nepřeskočitelné |  2 až 4 midrolls s duration odpovídající 720 sekund do hodiny / 3.5 minuty nepřeskočitelné |


## Příklady volání pozic

#### Varianta 2 - TV - VOSDAL T3 - preroll

`https://a.iprima.cz/dserver/site=Nazevoperatora_KANAL/section=smart_tv/area=preroll-1/size=spot,preroll/duration=30/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=clipid,productid/showname=nazev_poradu/viewid=[random]/random=[random]/operator=Nazevoperatora/variant=varianta2/broadcasting=vosdalt3/clipid=rE49300/productid=/skip=1/gdpr=1/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/`

#### Varianta 2 - TV - VOSDAL T3 - midroll

`https://a.iprima.cz/dserver/site=Nazevoperatora_KANAL/section=smart_tv/area=midroll-1/size=spot,midroll/duration=30/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=clipid,productid/showname=nazev_poradu/viewid=[random]/random=[random]/operator=Nazevoperatora/variant=varianta2/broadcasting=vosdalt3/clipid=rE49300/productid=/skip=1/gdpr=1/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/`

#### Varianta 2 - Mobile - živé vysílání - midroll

`https://a.iprima.cz/dserver/site=Nazevoperatora_KANAL/section=mobile/area=midroll-1/size=spot,midroll/duration=360/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=clipid,productid/showname=nazev_poradu/viewid=[random]/random=[random]/operator=Nazevoperatora/variant=varianta2/broadcasting=livestream/clipid=rE49300/productid=/skip=1/gdpr=1/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/`

#### Varianta 2 - Mobile - VOSDAL T3 - midroll

`https://a.iprima.cz/dserver/site=Nazevoperatora_KANAL/section=mobile/area=midroll-1/size=spot,midroll/duration=360/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=clipid,productid/showname=nazev_poradu/viewid=[random]/random=[random]/operator=Nazevoperatora/variant=varianta2/broadcasting=vosdalt3/clipid=rE49300/productid=/skip=1/gdpr=1/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/`



        gantt
        dateFormat  YYYY-MM-DD
        title Adding GANTT diagram functionality to mermaid

        section A section
        Completed task            :done,    des1, 2014-01-06,2014-01-08
        Active task               :active,  des2, 2014-01-09, 3d
        Future task               :         des3, after des2, 5d
        Future task2               :         des4, after des3, 5d

        section Critical tasks
        Completed task in the critical line :crit, done, 2014-01-06,24h
        Implement parser and jison          :crit, done, after des1, 2d
        Create tests for parser             :crit, active, 3d
        Future task in critical line        :crit, 5d
        Create tests for renderer           :2d
        Add to mermaid                      :1d

        section Documentation
        Describe gantt syntax               :active, a1, after des1, 3d
        Add gantt diagram to demo page      :after a1  , 20h
        Add another diagram to demo page    :doc1, after a1  , 48h

        section Last section
        Describe gantt syntax               :after doc1, 3d
        Add gantt diagram to demo page      : 20h
        Add another diagram to demo page    : 48h