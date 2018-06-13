⚠️ **TODO**

To configure Google Publisher Tags (GPT) and Prebid, you create one central configuration object which you pass as
_config_ prop to the [[AdvertisingProvider|API#advertisingprovider]] component.

The overall structure of your configuration is as follows:

```javascript
const config = {
  // global ad unit path, used if not specified in a slot (string)
  path,

  // global GPT targeting parameters (object)
  targeting,

  // GPT size mapping definitions
  sizeMappings,

  // global Prebid configuration
  prebid,

  // configuration of custom events dispatched by the creatives
  customEvents,

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

      // slot size configuration for GPT
      sizeMappingName,
      
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
};
```

## Global GPT Configuration

### Path

`config.path`

The ad unit path that is used for defining GPT slots, if there is no path specified in the GPT slot config.

**Type**: String

**Example**:

```javascript
const config = {
  path: "/19968336/header-bid-tag-0"
};
```

### Targeting

`config.targeting`

An object of arbitrary parameters that are used to define global targeting parameters for the slots using
[pubads.setTargeting](https://developers.google.com/doubleclick-gpt/reference#googletag.PubAdsService_setTargeting).

Note that you can also define targeting parameters for individual slots (see section [Slot Configuration](#slot-path) below).

**Type**: Object

**Example:**

```javascript
const config = {
  targeting: {
    gender: "female"
  }
};
```

**See also**:
[!googletag.PubAdsService setTargeting(key, value)](https://developers.google.com/doubleclick-gpt/reference#googletag.PubAdsService_setTargeting)
(GPT reference)

### Size Mappings

`config.sizeMappings`

An object with size mapping definitions to be used for responsive ads.

The object's keys are the size mapping names referenced from the configuration of each slot 

## Custom Events 

`config.customEvents`

## Global Prebid Configuration

`config.prebid`

## Slot Configuration

`config.slots`

### ID

`config.slots.id`

### Slot Path

`config.slots.path`

### Collapse Empty Div

`config.slots.collapseEmptyDiv`

### Slot Targeting

`config.slots.targeting`

### Sizes

`config.slots.sizes`

### Size Mapping Name

`config.slots.sizeMappingName`

## Slot Prebid Configuration

`config.slots.prebid`

### Prebid Media Types

`config.slots.prebid.mediaTypes`

### Bids

`config.slots.prebid.bids`

#### Bidder

`config.slots.prebid.bids.bidder`

#### Bid Parameters

`config.slots.prebid.bids.params`

