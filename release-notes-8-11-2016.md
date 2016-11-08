### API release note staging 8-10-2016

#### Direct prepaid streaming

- https://github.com/dropininc/dropin-api-v2/pull/400 -> Direct prepaid streaming pricing, update 1
- https://github.com/dropininc/dropin-api-v2/pull/406 -> Direct preapid streaming pricing, update 2
- https://github.com/dropininc/dropin-api-v2/pull/417 -> Fix bug for direct pricing
- https://github.com/dropininc/dropin-api-v2/pull/441 -> Fix bug authorization amount for insurance, drone, direct, direct-prepaid
- https://github.com/dropininc/dropin-api-v2/pull/436 -> Fix bug returning photos in direct streaming when resuming stream
- https://github.com/dropininc/dropin-api-v2/pull/437 -> Fix bug for stopping direct streaming
- https://github.com/dropininc/dropin-api-v2/pull/423 -> Fix bug for direct prepaid
- https://github.com/dropininc/dropin-api-v2/pull/424 -> Fix bug pricing not correct for direct prepaid when stopping from operator

https://dropin.atlassian.net/browse/DDIRECT-227
https://dropin.atlassian.net/browse/DDIRECT-194
https://dropin.atlassian.net/browse/DDIRECT-267
https://dropin.atlassian.net/browse/DDIRECT-266
https://dropin.atlassian.net/browse/DDIRECT-248


#### Improvement on getting gigs for resuming

- https://github.com/dropininc/dropin-api-v2/pull/434 -> Improvement on getting streams for resuming.
https://dropin.atlassian.net/browse/DROPIN-455

#### Complete and fix bug for global config for streaming where appropriate.

- https://github.com/dropininc/dropin-api-v2/pull/425 -> API for getting/setting price config.
- https://github.com/dropininc/dropin-api-v2/pull/442 -> Fix bug for API getting/setting price config.
- https://github.com/dropininc/dropin-api-v2/pull/414 -> Fix bug on 
https://dropin.atlassian.net/browse/DROPIN-470
https://dropin.atlassian.net/browse/DRONE-25
https://dropin.atlassian.net/browse/DRONE-24

#### Refine on searching operator's rules

https://github.com/dropininc/dropin-api-v2/pull/431 -> Refine searching operator
https://dropin.atlassian.net/browse/DESKTOP-658

#### More improvements
- https://github.com/dropininc/dropin-api-v2/pull/428
https://docs.google.com/spreadsheets/d/1J92JNYugICxktrMSMmkQ2oHyd7QC2Tv7qDtzKs0y7ww/edit#gid=540704245 item 3

- https://github.com/dropininc/dropin-api-v2/pull/414
Fix bug on redirecting flow for insurance app and drone app, firstly, we had wanted insurance viewer to request to insurance operator AND drone operator, after that, we decided to insurance viewer to request to insurance app OR drone app, base on setting of the flag.




