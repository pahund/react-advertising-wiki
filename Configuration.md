## Contents

* [Overview](#overview)
* [Global GPT Configuration](#global-gpt-configuration)
  * [Path](#path)
  * [Targeting](#targeting)
  * [Size Mappings](#size-mappings)
  * [Custom Events](#custom-events)
* [Global Prebid Configuration](#global-prebid-configuration)
* [Slot GPT Configuration](#slot-gpt-configuration)
  * [ID](#id)
  * [Slot Path](#slot-path)
  * [Collapse Empty Div](#collapse-empty-div)
  * [Slot Targeting](#slot-targeting)
  * [Sizes](#sizes)
  * [Size Mapping Name](#size-mapping-name)
* [Slot Prebid Configuration](#slot-prebid-configuration)
  * [Prebid Media Types](#prebid-media-types)
  * [Bids](#bids)
    * [Bidder](#bidder)
    * [Bid Parameters](#bid-parameters)

## Overview

To configure Google Publisher Tags (GPT) and Prebid, you create one central configuration object which you pass as
_config_ prop to the [[AdvertisingProvider|API#advertisingprovider]] component.

The overall structure of your configuration is as follows:

```javascript
const config = {
  // global ad unit path, used if not specified in a slot (string)
  path,

  // global GPT targeting parameters (object)
  targeting,

  // GPT size mapping definitions (object)
  sizeMappings,

  // global Prebid configuration (object)
  prebid,

  // configuration of custom events dispatched by the creatives (object)
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

Hint: For an example of a complete, valid configuration, refer to
[testAdvertisingConfig.js](https://github.com/technology-ebay-de/react-prebid/blob/master/src/utils/testAdvertisingConfig.js)
in the source code, which is used for the unit tests of _react-prebid_.

## Global GPT Configuration

### Path

`config.path`

The ad unit path that is used for defining GPT slots, if there is no path specified in the GPT slot config.

You can also paths for individual slots (see section [Slot Path](#slot-path) in section “Slot GPT Configuration” below).

**Type**: String

**Example**:

```javascript
const config = {
  path: "/19968336/header-bid-tag"
};
```

### Targeting

`config.targeting`

An object of arbitrary parameters that are used to define global targeting parameters for the slots using
[pubads.setTargeting](https://developers.google.com/doubleclick-gpt/reference#googletag.PubAdsService_setTargeting) of
the GPT API.

Note that you can also define targeting parameters for individual slots (see section [Slot Targeting](#slot-targeting)
in section “Slot GPT Configuration” below).

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

The object's keys are the size mapping names referenced from the configuration of each slot (see section
[Size Mapping Name](#size-mapping-name) in section “Slot GPT Configuration” below).

The object's values are arrays of objects with two properties:

* `viewPortSize`: The target view port size of the size mapping; array with two numbers for width and height
* `sizes`: Array of ad sizes to be displayed in this view port; array of arrays with two numbers for width and height

**Type**: Object

**Example**:

```javascript
const config = {
  sizeMappings: {
    phone: [
      {
        viewPortSize: [320, 700],
        sizes: [[300, 250], [320, 50]]
      }
    ]
  }
};
```

**See also**:
[Build Responsive Ads](https://support.google.com/dfp_premium/answer/3423562?visit_id=1-636644784470043770-1893849263)
(Google DoubleClick for Publishers documentation)

### Custom Events

`config.customEvents`

Custom events allow you to pass callback functions to your [[AdvertisingSlot|API#advertisingslot]] components using the
_customEventHandlers_ prop.

With these callback functions, you can trigger layout changes using events dispatched from the iframes delivered by the
ad server. This is useful, for example, to set the page's background color to match a creative.

**Type**: Object

**Example**:

```javascript
const config = {
  customEvents: {
    blueBackground: {
      eventMessagePrefix: "BlueBackground:"
    }
  }
};
```

**See also**: [[Custom Events]]

## Global Prebid Configuration

`config.prebid`

Allows you to specify global configuration options for Prebid.js, which are passed along using
[pbjs.setConfig](http://prebid.org/dev-docs/publisher-api-reference.html#module_pbjs.setConfig).

**Type**: Object

**Example**:

```javascript
const config = {
  prebid: {
    bidderTimeout: 1500,
    priceGranularity: "high",
    bidderSequence: "fixed"
  }
};
```

**See also**: [pbjs.setConfig](http://prebid.org/dev-docs/publisher-api-reference.html#module_pbjs.setConfig) (Prebid.js
API reference)

## Slot GPT Configuration

`config.slots`

An array of slot config objects, which are explained in the following sub-sections.

**Type**: Array of objects

### ID

`config.slots.id`

The element ID of the div in your HTML code that is filled with the creative from the ad server. This ID corresponds to
the _id_ prop of the [[AdvertisingSlot|API#advertisingslot]] component.

**Required**

**Type**: String

**Example**:

```javascript
const config = {
  slots: [
    {
      id: "div-gpt-ad-1460505748561-0"
    }
  ]
};
```

**See also**: [[AdvertisingSlot|API#advertisingslot]] component

### Slot Path

`config.slots.path`

The ad unit path for this specific slot. If it is omitted, the [ad unit path from the global GPT configuration](#path)
is used. You _must_ have either a slot specific or global ad unit path.

**Type**: String

**Example**:

```javascript
const config = {
  slots: [
    {
      path: "'/19968336/header-bid-tag-0"
    }
  ]
};
```

**See also**: [global ad unit path](#path)

### Collapse Empty Div

`config.slots.collapseEmptyDiv`

Use this to remove div elements from the DOM when the ad server has no matching ad to serve for this slot.

The value is an array of one or two booleans, which determine the collapsing of empty div elements as follows:

* _First bool_: Whether to collapse the slot if no ad is returned
* _Second bool_: Whether to collapse the slot even before an ad is fetched; ignored if collapse is not true

**Type**: Array of booleans

**Example**:

```javascript
const config = {
  slots: [
    {
      collapseEmptyDiv: [true, false]
    }
  ]
};
```

**See also**:
[!googletag.Slot setCollapseEmptyDiv(collapse, opt_collapseBeforeAdFetch)](https://developers.google.com/doubleclick-gpt/reference#googletag.Slot_setCollapseEmptyDiv)
(GPT reference)

### Slot Targeting

`config.slots.targeting`

An object of arbitrary parameters that are used to define targeting parameters for the slot using
[slot.setTargeting](https://developers.google.com/doubleclick-gpt/reference#googletag.Slot_setTargeting) of the GPT API.

Note that you can also define global targeting parameters for all slots (see section [Targeting](#targeting) in section
“Global GPT Configuration” below).

**Type**: Object

**Example:**

```javascript
const config = {
  slots: [
    {
      targeting: {
        age: 27
      }
    }
  ]
};
```

**See also**:
[!googletag.Slot setTargeting(key, value)](https://developers.google.com/doubleclick-gpt/reference#googletag.Slot_setTargeting)
(GPT reference)

### Sizes

`config.slots.sizes`

Define one or more sizes for the ad slot. This is the size that is used in the ad request if no responsive size mapping
is provided or the size of the viewport is smaller than the smallest size provided in the mapping.

**Note:** More than one sizes is a DFP premium feature; if you use DoubleClick for Publishers Small Business, you can
only specify one size.

**Required**

**Type**: Array of arrays (see examples)

**Examples:**

_One size (array with width and height)_

```javascript
const config = {
  slots: [
    {
      sizes: [320, 50]
    }
  ]
};
```

_Multiple sizes_

```javascript
const config = {
  slots: [
    {
      sizes: ["fluid", [300, 250], [320, 50], [320, 100]]
    }
  ]
};
```

**See also**:
[googletag.Slot googletag.defineSlot(adUnitPath, size, opt_div)](https://developers.google.com/doubleclick-gpt/reference#googletag.defineSlot)
(GPT reference)

### Size Mapping Name

`config.slots.sizeMappingName`

The size mapping for responsive ads to be used for this slot. Corresponds to one of the size mappings you have
configured in the [global GPT configuration](#size-mappings).

**Type**: String

**Example**:

```javascript
const config = {
  slots: [
    {
      sizeMapping: "phone"
    }
  ],
  sizeMappings: {
    phone: [
      {
        viewPortSize: [320, 700],
        sizes: [[300, 250], [320, 50]]
      }
    ]
  }
};
```

**See also**: [Size Mappings](#size-mappings)

## Slot Prebid Configuration

`config.slots.prebid`

⚠️ **TODO**

### Prebid Media Types

`config.slots.prebid.mediaTypes`

⚠️ **TODO**

### Bids

`config.slots.prebid.bids`

⚠️ **TODO**

#### Bidder

`config.slots.prebid.bids.bidder`

⚠️ **TODO**

#### Bid Parameters

`config.slots.prebid.bids.params`

⚠️ **TODO**

---

← [[API]] | [[Custom Events]] →
