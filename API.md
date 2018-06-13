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
| `config`  | [AdvertisingConfigPropType](https://github.com/technology-ebay-de/react-prebid/blob/master/src/components/utils/AdvertisingConfigPropType.js) | Yes, if active | –       | Refer to section [[Configuration]] for details                                       |
| `plugins` | Array                                                                                                                                         | –              | –       | Refer to section [[Plugins]] for details                                             |

### Usage

```javascript
import React from "react";
import { AdvertisingProvider } from "react-prebid";

export default ({ children }) => <AdvertisingProvider active={false}>{children}</AdvertisingProvider>;
```

Refer to the [[Usage|Usage#adding-the-provider]] page for a more elaborate example.

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

← [[Usage]] | [[Configuration]] →
