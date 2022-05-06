# Online advertising with IPTV operators

#### Description

Requests to the advertising system are made via HTTPS protocol and GET call type without using a proxy. The query to the advertising system must be done in real time according to predefined markers set by the operator and in case of TV break substitution, it follows the FTV Prima API and timestamp. When substituting, it is necessary to take into account the delay according to the type of transmission. The total length of the commercial break, passed in the duration parameter, must also be part of the query, based on which the advertising system returns the corresponding set of commercial spots.


## Ad server query specification 

#### Request

| Name | Value |
| ------ | ------ |
| Protokol | HTTPS |
| Typ | GET |
| Síť | Internet - bez proxy |


#### Response

| Name | Value |
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

#### Response with CORS

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

#### Errors

| Type | Description |
| ------ | ------ |
| 404 Invalid Job ID | When the specified job ID does not exist. |
| 408 Request Timeout | When the engine fails to respond in time to a request. |
| 429 Too Many Requests | When the server receives too many requests from a visitor. The response is sent until the condition subsides |
| 500 Internal Server Error | When there is an uncategorized error. |

#### The advertising system response is in VAST 3.0 format

The ad system's answer is VAST 3.0 with multi AdPods. The processing of each video spot on the player side must meet this standard as a minimum. 

------

## List of ad query parameters


| Description | Parameter | Value | Details |
| ------ | ------ | ------ | ------ |
| Domain | - | a.iprima.cz | doména pro provolání dotazu |
| Type server | - | dserver | určení výdejového serveru |
| Channel | site | Nazevoperatora_KANAL | predefinovaný, složený z názvu operátora a názvu kanálu |
| Device | section | smart_tv,mobile,web_desktop, web_mobile, mobile_tablet,web_tablet | určení zařízení |
| Type ad placemment | area | preroll-1, midroll-1, midroll-2, midroll-3, midroll-4 | určení typu breaku |
| Type size | size | kombinace - spot,preroll  / spot,midroll | fixni hodnota |
| Length of break | duration | 30, 360, dle délky tv breaku | fixní hodnota dle typu, při nahrazování simulcastu dle délky tv breaku |
| Validation of VAST 3 | format | validvast3 | fixní hodnota |
| Template | formatmt | application%2Fxml | fixní hodnota |
| S2S declaration | SUPERTAG | InstreamVideo | fixní hodnota |
| Type of content | keyword | porad,typ | multivalue, alfanumeric, oddělovač čárka |
| Name of content | showname | nazev_poradu | z api Prima, ošetření formátu bez diakritiky, odstranění speciálních znaků, mezera, pomlčka je nahrazena podtržítkem |
| Audio settings| mute | 1 / 0 | předání informace, jesti má přehrávač při startu reklamního blocku aktivní zvuk |
| Unique id of break | viewid | random hodnota | hash, 10ti místné číslo , unikátní v rámci volání breaku, zamezení duplicity spotů |
| Cachebuster | random | random hodnota | hash, 10ti místné číslo  |
| Device id  | mid | nazevoperatora_hashID | pokud má operátor vlastní určtení unikátního zařízení, složený z názvu operátora a zahashovaného id zařízení |
| Ad segments | seg1 až segN | - | hodnota bude doplněna po vzájemné konzultaci |
| Name of klient | operator | Nazevoperatora | predefinovaný, alfanumeric |
| Type of variant | variant | varianta2,varianta3 | určení typu varianty, podle které se účtuje |
| Type of broadcasting | broadcasting | livestream,startover,vosdalt3,ts47,vosdalt7 | určení vysílacího modelu |
| Clip id | clipid | - | interní párovací číslo média - rE37563 |
| Product id | productid | - | unikátní výrobní číslo |
| Skippable | skip | 1 / 0 | z api Prima |
| GDPR | gdpr | 1 | fixní hodnota |
| Consent | consent | tcstring base 64 | Operátor předá souhlas od uživatele dle TFC v2 - formát tcString |


#### List of channel names for the site parameter

Important! - Name combinations must be predefined in the advertising system. The setting of the names used will be in agreement with FTV Prima.

| Channel | Client name plus channel |
| ------ | ------ |
| PRIMA | Clientname_PRIMA |
| COOL | Clientname_COOL |
| MAX | Clientname_MAX |
| ZOOM | Clientname_ZOOM |
| LOVE | Clientname_LOVE |
| KRIMI | Clientname_KRIMI |
| STAR | Clientname_STAR |
| CNN | Clientname_CNN |
| PRIMAPLUS | Clientname_PRIMAPLUS |
| SHOW | Clientname_SHOW |


#### Standard VAST 4.2

Ad processing on the player side must be according to the iab VAST 4.2 standard - https://iabtechlab.com/wp-content/uploads/2019/06/VAST_4.2_final_june26.pdf


## Examples

#### Preroll

`https://a.iprima.cz/dserver/site=Nazevoperatora_KANAL/section=smart_tv/area=preroll-1/size=spot,preroll/duration=30/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=clipid,productid/showname=nazev_poradu/viewid=[random]/random=[random]/operator=Nazevoperatora/variant=varianta2/broadcasting=vosdalt3/clipid=rE49300/productid=/skip=1/gdpr=1/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/`

#### Midroll

`https://a.iprima.cz/dserver/site=Nazevoperatora_KANAL/section=smart_tv/area=midroll-1/size=spot,midroll/duration=30/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=clipid,productid/showname=nazev_poradu/viewid=[random]/random=[random]/operator=Nazevoperatora/variant=varianta2/broadcasting=vosdalt3/clipid=rE49300/productid=/skip=1/gdpr=1/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/`

