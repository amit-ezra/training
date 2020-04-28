# Example HLD

## Contents

* [Overview](#overview)
* [Requirements](#requirements)
* [API](#api) 
* [Security](#security) 
* [Deployement](#deployment) 
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
 
 
### Direct Access to ElasticSearch
If enabled the microservice will allow tunneling and direct access to Elastic Search. This means that any request that is not covered by the above API, will just be re-directed directly there. 


![diagram](http://www.plantuml.com/plantuml/svg/3SKn3W9120NGtbFe0HnwUqkhFG4YsI4PcCqVqCJJwslUnK96lRGmpZtpM3SYyAVjbhsUjHGo8pMooNHYjxpg5qm9jh3OoNcbWkxZlycc3EaF4ynDyJRHTepoqmy0)


## Security

Authentication for access to the microserivice will be done by passing an OAuth Authentication header with a Bearer token. The header will be validated using third party OKTA microservice

Access directly to ElasticSearch will be limited by the network level to be allowed only from the microservice using AWS security group.

## Deployment
First stage - deploy the new MS using docker on ECS. Run sanity and make sure everything is working.
Second stage - Enable allow passthrough requests to ES. Run Sanity and make sure it's working.
Third Stage - update configuration for all Emedgene services to use the new MS instead of directly ES \ change DNS endpoint to point to the MS instead of ES. 
Fourth stage - Close direct access to ES.
Fifth stage - optional - Disable passthrough requests to ES from the MS

## Backward Compatibility
While passthrough requests are enabled, we maintine backward compatibility. Once we disable it backward compatibility is disabled. 



