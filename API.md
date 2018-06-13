## Contents

* [AdvertisingProvider](#advertisingprovider)
* [AdvertisingSlot](#advertisingslot)
* [connectToAdServer](#connecttoadserver)

## AdvertisingProvider

The *AdvertisingProvider* component acts as a wrapper around a React layout that contains *AdvertisingSlot*, which display the ads.

It handles all the interaction with the DFP ad server and is required for the advertising slots to work.

### Props

| Name | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `active` | Boolean | – | `true` | If set to `false`, the advertising provider is deactivated and no ads will be served |
| `config` | [AdvertisingConfigPropType](https://github.com/technology-ebay-de/react-prebid/blob/master/src/components/utils/AdvertisingConfigPropType.js) | Yes, if `active === true` | – | Refer to section [[Configuration]] for details |
| `plugins` | Array | – | – | Refer to section [[Plugins]] for details |

## AdvertisingSlot


## connectToAdServer


---

← [[Usage]] | [[Configuration]] →

