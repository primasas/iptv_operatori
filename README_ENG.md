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

#### The advertising system response is in VAST 4.2 format

The ad system's answer is VAST 4.2 with multi AdPods. The processing of each video spot on the player side must meet this standard as a minimum. 

------

## List of ad query parameters


| Meaning | Parameter | Value | Description |
| ------ | ------ | ------ | ------ |
| operator name | operator | operator name | predefined, alphanumeric |
| Channel | site | Nazevoperatora_KANAL | predefined, alphanumeric operator name and channel name |
| Device | section | smart_tv, mobile, web_desktop, web_mobile, mobile_tablet, web_tablet | device designation |
| ad position type | area | preroll-1, preroll-7, midroll-1 | break type / preroll-1 (6Os) / preroll-7 (tv break duration) / midroll-1 (120s, 75s)|
| ad type | size | combination - spot,preroll / spot,midroll | fixed value |
| break length | duration | 30, 150, 135, 105, 90, according to tv break length | fixed value according to type, when replacing simulcast according to tv break length |
| Validation on VAST 3 | format | validvast3 | fixed value |
| Format settings | formatmt | application%2Fxml | fixed value |
| Internal declaration for S2S | SUPERTAG | InstreamVideo | fixed value |
| Additional content data | keyword | from API: name, Clip_id, Product_id | multivalue, alphanumeric, comma separator |
| show name | showname | show_name | from Prima api, format treatment without diacritics, removal of special characters, space, hyphen replaced by underscore, lowercase only |
| Cachebuster | random | random value | hash, 10-digit number |
| break marking | viewid | random value | hash, 10-digit number , unique within the break call, avoid duplicate spots |
| Device id | mid | operator_hashID | if the operator has its own unique device designation, consisting of the operator name and the hashed device id |
| Advertising segments | seg1 to segN | - | value will be added after mutual consultation |
| Variant Type | Variants | Variant2, Variant3 | determination of the type of variant to be charged |
| broadcasting type | broadcasting | livestream, startover, vosdalt3,ts47 | determination of the broadcasting model |
| Clip id | clipid | - | internal media pairing number - rE37563 - available in Prima api |
| Product id | productid | - | unique serial number. Slashes must be replaced with a dash! - available in Prima api |
| Consent | consent | tcstring base 64 | Operator passes consent from user according to TFC v2 - tcString format |
| GDPR activation | gdpr | 1 / 0 | fixed value | If the operator is not able to pass consent, the value must be 0. |



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

### TV device VOSDAL-T3
#### Preroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=smart_tv/area=preroll-1/size=spot,preroll/duration=30/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=vosdalt3/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

#### Midroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=smart_tv/area=midroll-1/size=spot,midroll/duration=120/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=vosdalt3/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`


### TV device TS 4-7
#### Preroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=smart_tv/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=ts47/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

#### Midroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=smart_tv/area=midroll-1/size=spot,midroll/duration=120/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=ts47/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`


### APP - Simulcast

#### Midroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=midroll-7/size=spot,midroll/duration=360/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=livestream/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

### APP - Start-over

#### Preroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=startover/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

#### Midroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=midroll-7/size=spot,midroll/duration=360/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=startover/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

### APP - VOSDAL T-3

#### Preroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=vosdalt3/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`
#### Midroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=midroll-1/size=spot,midroll/duration=120/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=vosdalt3/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

### APP - TS 4-7

#### Preroll
`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=preroll-1/size=spot,preroll/duration=60/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=ts47/clipid=clipid/productid=productid/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`

#### Midroll

`https://a.iprima.cz/dserver/operator=Nazevoperatora/site=Nazevoperatora_KANAL/section=mobile/area=midroll-1/size=spot,midroll/duration=75/format=validvast3/formatmt=application%2Fxml/SUPERTAG=InstreamVideo/keyword=pocasi_rezerva,PC220112,22-311-00109-0112/showname=pocasi_rezerva/random=1234567890/viewid=1234567890/variant=varianta2/broadcasting=ts47/clipid=PC220112/productid=22-311-00109-0112/consent=CPRUH0OPRUH0OAHABBENB5CgAP_AAH_AAAAAHfoBpDxkBSFCAGJoYtkgAAAGxwAAICACABAAoAAAABoAIAQAAAAQAAAgBAAAABIAIAIAAABAGEAAAAAAQAAAAQAAAEAAAAAAIQIAAAAAAiBAAAAAAAAAAAAAAABAQAAAgAAAAAIAQAAAAAEAgAAAAAAAAAAABAAAAAgd1AoAAWABUAC4AHAAQAAyABoADmAIgAigBMACeAFUALgAXwAxAB-AEJAIgAiQBSgCxAGWAM2AdwB3gD9AIQARYAtIBdQDAgGsAOoAfIBIICbQFqALzAZIA0oBqYDugAAA.f_gAD_gAAAAA/gdpr=1/`



