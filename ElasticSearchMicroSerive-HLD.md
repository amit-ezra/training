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
### swagger -  https://app.swaggerhub.com/apis/amit-ezra/exemple-hld/1.0.0#/ 
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


![diagram](http://www.plantuml.com/plantuml/png/lPHRRzD048NVzrUC_1AmGfnUk50rLIyg85Kbg4df0qBMn3jDLbtlcjrrHAduxumtCQHMNa2ebQfuPuxcEtioyR4Fw49TQY4gVb25gj04IAudvrS9e8VQatlSEf_0fIgSdP1RgeAYglGTZKRF9vWGkc8H9TIHZ2-c9x5Xxm8A8O8Ack36gK1mXfxWCph5IY-m3XQBe9R8BMk468tWutZA0gqaND6tcdo0qKY8nYoyE68x1v2cdyQJQPf2acJBzQZi_FBzpkxU_ijNhz-yJH8GpjX0uDHy4LOtJTC5eIJdqrG0dDHXOPtwWK5P0wU4ZXnasU7Ktf4voZAMLtYlBSeeVqXQK780NxHYL_OMsXf833WO3BuACX8Qi12bPxinWGt36JAM34U5WHvPBFrV6jn0EGgrCx0x739luxW3uulM27LoKqRIzooVrVekIvAkbUxbyoZdgn_6XwC-Lo79mtr5N6ooGTLaiwxRwdUhS8I1agR-SyEoWCvVRuNe7K3yx6yHD2EvtsjJxsqJCRhLhT1RLKgom9E-7UJvd0mv5kRakPxdpzRp-r2WfznWINtmM5PwTNY-RQ2tjIiQQkLiGTwJpDTZHFSBYKj-uwoPmvaZsMm3QX-N_Ccs_1Gbj8lq13kx7TVciZ_SWtVMpPJaCBPNOsQNodjbvf427oBsdruD6uWTk3QuDaqoKaxn_xrukZ-bXvb6uF-D3Fvu6SACqMw5viOnY-fI_m80)


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



