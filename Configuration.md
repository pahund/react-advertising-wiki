⚠️ **TODO**

To configure Google Publisher Tags (GPT) and Prebid, you create one central configuration object which you pass as *config* prop to the [[AdvertisingProvider|API#advertisingprovider]] component.

The overall structure of your configuration is as follows:

```javascript
const config = {
    // global ad unit path, used if not specified in a slot (string)
    path,

    // global GPT targeting parameters (object)
    targeting,

    // global Prebid configuration
    prebid,

    // GPT size mapping definitions
    sizeMappings,

    // configuration of custom events dispatched by the creatives
    customEvents

    // array of slot configurations
    slots: [
        {
            // slot ID
            id,

            // slot's ad unit path
            path,

            // configuration for removing empty slots
            collapseEmptyDivs,

            // targeting parameters specific to the slot
            targeting,

            // slot size configuration for GPT
            sizes,

            // array with slot's Prebid configuration objects
            prebid: [
                {
                    // media types object for this Prebid config
                    mediaTypes,

                    // array of bid objects
                    bids: [
                        {
                            // bidder ID
                            bidder,

                            // object with bidder-specific config
                            params
                        }
                    ]
                }
            ]
        }
    ]
}
```

## Global Configuration

### config.path

The ad unit path that is used for defining GPT slots, if there is no path specified in the GPT slot config.

**Type**: String

**Example**: 

```javascript
const config = {
    path: '/19968336/header-bid-tag-0'
}
```

### config.targeting

An object of arbitrary parameters that are used to define global targeting parameters for the slots using [googletag.pubads.setTargeting](https://developers.google.com/doubleclick-gpt/reference#googletag.PubAdsService_setTargeting).

**Type**: Object

**Example:**

```javascript
const config = {
    targeting: {
        gender: 'female'
    }
}
```

## Slot Configuration