# Example HLD

## Contents

* [Overview](#overview)
* [Requirements](#requirements)
* [API](#api) 
* [Security](#security) 
* [Deployement](#deployement) 
* [Backward Compatibility](#backward-compatibility) 


## Overview 
As part of improving our preformace and stability we decided to wrapp our Elastic Search with a microservice that will manage all reading and writing requests. 
The added benefit of such microservice, is that it allows us to better manage R&W throtteling, and archive indexes.



## Requirements
* Create a new micro service Elastic search that throttle writing requests to one request at a time
* API for writing new varaints to an index
* API for reading variants from an index
* API for deleting a case from index
* API for throtteling 
* Cost optimizations by archiving old indexes


## API 

### New Case
**POST** /v1/variants/EMG1235678
Headers: Authorization: Bearer <token>
Payload: {"variants": [<list of variants data>]}

Responses:
201 OK
403 Forbidden
401 Authentication Required
400 Bad Request

### Update Case
**PUT** /v1/variants/EMG1235678
Headers: Authorization: Bearer <token>
Payload: {"variants": [<list of variants data>]}

Responses:
201 OK
403 Forbidden
401 Authentication Required
400 Bad Request

### Read Case variants
**GET** /v1/variants/EMG1235678
Headers: Authorization: Bearer <token>

Responses:
200 OK
{"variants": [<list of variants data>]}

403 Forbidden
401 Authentication Required
400 Bad Request


### Delete Case
**DELETE** /v1/variants/EMG1235678
Headers: Authorization: Bearer <token>
Payload: {"variants": [<list of variants data>]}

Responses:
201 OK
403 Forbidden
401 Authentication Required
400 Bad Request



![diagram](http://www.plantuml.com/plantuml/svg/1S513WCX20NGVK_H7g2oxsse4YKHDV3997FwUU-ZgyviaZxV0pZn8tA-IbUC_6U8rxqW2wLk8pR5LsvdWWJ8E21EJRaxMpapxVK0)
test
