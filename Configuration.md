⚠️ **TODO**

To configure Google Publisher Tags (GPT) and Prebid, you create one central configuration object which you pass as *config* prop to the [[AdvertisingProvider|API#advertisingprovider]] component.

The overall structure of your configuration is as follows:

```javascript
{
    // global ad unit path, used if not specified in a slot (string)
    path,

    // global GPT targeting parameters (object)
    targeting,

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

                }
            ]
        }
    ],
    prebid,
    sizeMappings,
    customEvents
}
```