## Contents

* [Passing the Configuration Asynchronously](#passing-the-configuration-asynchronously)
* [Updating the Configuration After Initial Rendering](#updating-the-configuration-after-initial-rendering)

## Passing Configuration Asynchronously

🎁 **since version [1.1.0](https://github.com/technology-ebay-de/react-advertising/releases/tag/1.1.0)**

Instead of rendering your <a href="https://github.com/technology-ebay-de/react-advertising/wiki/API#advertisingprovider">AdvertisingProvider</a> component with a configuration prop, you can choose to omit the config prop.

**Why is this useful?**

In some cases, you may want not have your ad config ready when the page is initially loaded, perhaps because you are fetching it asynchronously from a service. In this case, you wouldn't want to block the whole page from being shown to the user, just because the ad config has not been loaded yet.

The solution is to render the `AdvertisingProvider` without a config on initial page load, then load the config, then re-render the page after the config has been loaded successfully.

## Updating the Configuration After Initial Rendering

🎁 **since version [2.0.0](https://github.com/technology-ebay-de/react-advertising/releases/tag/2.0.0)**

You can update your <a href="https://github.com/technology-ebay-de/react-advertising/wiki/API#advertisingprovider">AdvertisingProvider</a> by re-rendering it with a different configuration prop than before.

When the `AdvertisingProvider` receives a new `config` prop, it will automatically tear down GPT and Prebid and set them up again with the updated configuration, causing all the ad slots in the container to get refreshed with new ads according to the new config.

**Why is this useful?**

Targeting parameters may change due to user interactions, for example when the user logs in, you now know a lot more about them than before. Your ads will yield more revenue with this additional information, which you want to pass as targeting parameters. Passing an updated configuration to the `AdvertisingProvider` allows you to refresh your ads slots with ads that are more specifically targeted.

---

← [[Internet Explorer 11 Compatibility]] | [[API]] →