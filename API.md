## Contents

* [AdvertisingProvider](#advertisingprovider)
* [AdvertisingSlot](#advertisingslot)
* [connectToAdServer](#connecttoadserver)

## AdvertisingProvider

The _AdvertisingProvider_ component acts as a wrapper around a React layout that contains _AdvertisingSlot_ components,
which display the ads.

It handles all the interaction with the DFP ad server and is required for the advertising slots to work.

### Props

| Name      | Type                                                                                                                                          | Required       | Default | Description                                                                          |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------- | -------------- | ------- | ------------------------------------------------------------------------------------ |
| `active`  | Boolean                                                                                                                                       | –              | `true`  | If set to `false`, the advertising provider is deactivated and no ads will be served |
| `config`  | [AdvertisingConfigPropType](https://github.com/technology-ebay-de/react-prebid/blob/master/src/components/utils/AdvertisingConfigPropType.js) | - | –       | Refer to section [[Configuration]] for details                                       |
| `plugins` | Array                                                                                                                                         | –              | –       | Refer to section [[Plugins]] for details                                             |

### Usage

```javascript
import React from "react";
import { AdvertisingProvider } from "react-prebid";

export default ({ children }) => <AdvertisingProvider active={false}>{children}</AdvertisingProvider>;
```

Refer to the [[Usage|Usage#adding-the-provider]] page for a more elaborate example.

### Advanced Usage

#### Passing the Config Prop Later

You may have noticed that the `config` prop is not required. If no config is passed to the `AdvertisingProvider`, it will simply do nothing.

Why is this useful? In some cases, you may want not have your ad config ready when the page is initially loaded, perhaps because you are fetching it asynchronously from a service. In this case, you wouldn't want to block the whole page from being shown to the user, just because the ad config has not been loaded yet.

The solution is to render the `AdvertisingProvider` without a config on initial page load, then load the config, then re-render the page after the config has been loaded successfully.

This feature was introduced in version 1.1.0.

#### Updating the Config Prop After Initial Render

You can update your `AdvertisingProvider` by re-rendering it with a different configuration prop than before.

When the `AdvertisingProvider` receives a new `config` prop, it will automatically tear down GPT and Prebid and set them up again with the updated configuration, causing all the ad slots in the container to get refreshed with new ads according to the new config.

Why is this useful?

Targeting parameters may change due to user interactions, for example when the user logs in, you now know a lot more about them than before. Your ads will yield more revenue with this additional information, which you want to pass as targeting parameters. Passing an updated configuration to the `AdvertisingProvider` allows you to refresh your ads slots with ads that are more specifically targeted.

## AdvertisingSlot

Each _AdvertisingSlot_ component provides a div element which is filled with an ad from the DFP ad server.

### Props 

| Name                  | Type   | Required | Default | Description                                                                        |
| --------------------- | ------ | -------- | ------- | ---------------------------------------------------------------------------------- |
| `id`                  | String | Yes      | –       | The div element's ID, used by DFP to render ads, references [[Configuration#id]] |
| `style`               | Object | –        | –       | Use this for styling the slot using CSS properties, e.g. to add a background color |
| `className`           | String | –        | –       | Use this for specifying a CSS class for styling the slot                           |
| `customEventHandlers` | Object | –        | –       | Refer to section [[Custom Events]] for details                                     |

### Usage

```javascript
import React from "react";
import { AdvertisingSlot } from "react-prebid";

export default () => <AdvertisingSlot id="my-id" />;
```

Refer to the [[Usage|Usage#adding-the-slots]] page for a more elaborate example.

## connectToAdServer

This utility function allows you to create your own advertising slot component instead of using the provided
[AdvertisingSlot](#advertisingslot) component, giving you more freedom in your implementation.

You can pass any React component to the function to create a React component that is connected to the ad server.

The React component you pass to the function needs to have an `activate` property, which is a function to be called, for
example, when the component mounts. Calling `activate` causes the DFP ad server to fill the slot with an ad.

The `activate` prop is created and passed to your component when you wrap it using `connectToAdServer`.

### Usage

Refer to the source code of the
[AdvertisingSlot](https://github.com/technology-ebay-de/react-prebid/blob/master/src/components/AdvertisingSlot.js)
component for a reference implementation.

Here is a minimal example:

```javascript
import React, { Component } from 'react';
import { connectToAdServer } from 'react-prebid';

class MyAdSlot extends Component {
    componentDidMount() {
        this.props.activate('my-ad-id');
    }
    render() {
        return <div id="my-ad-id" />;
    }
}

export default connectToAdServer(MyAdSlot);
```

---

← [[Advanced Usage]] | [[Configuration]] →