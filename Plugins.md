Plugins are a way to specify additional initalization logic that is executed by the
[[AdvertisingProvider|API#advertisingprovider]] component.

It is an advanced feature that you probably won't need.

A bit of background why this may be useful:

At eBay, we use _react-prebid_ to render ads for our German online community [MOTOR-TALK](https://www.motor-talk.de/).

For MOTOR-TALK, we render our React layout both on the client side and on the server side to make our website search
engine friendly.

We use [Redux](https://redux.js.org) for state management. Our Redux state must be serializable, so that we can pass it
from the server to the client for state hydration.

This means the Redux state cannot have any functions in it, since those cannot be serialized. It is generally considered
an anti-pattern to put functions in the Redux state.

The problem now is that our [[advertising configuration|Configuration]] is also stored in the Redux state.

For some bidders, we configure Prebid with a function that converts the currency of the bids' CPM (cost per mille) from
US$ to EUR. But we cannot store this function in our config, because of the serialization.

This is why we came up with the plugin mechanism:

In addition to the config prop, the _AdvertisingProvider_ has a _plugins_ prop that allows us to pass this additional
configuration. It looks like this:

```javascript
const plugins = [
  {
    setupPrebid() {
      if (!this.config.usdToEurRate) {
        return;
      }
      window.pbjs.bidderSettings = {
        appnexus: {
          bidCpmAdjustment(bidCpm) {
            return bidCpm * this.config.usdToEurRate;
          }
        }
      };
    }
  }
];

const config = {
    usdToEurRate: 0.8611,
    /* other config */
};

function App() {
    return (
        <AdvertisingProvider config={config} plugins={plugins}>
            {/* other components */}
        </AdvertisingProvider>
    )
}
```

* The plugin code is executed when the _AdvertisingProvider_ sets up Prebid; this is specified through the name of the
  method chosen for the plugin function
* You can define multiple plugins, each plugin for one or more setup or initialization phases; valid names are:
  * _setupPrebid_: initialization of Prebid when _AdvertisingProvider_ component mounts
  * _teardownPrebid_: Prebid is deactivated when _AdvertisingProvider_ component unmounts; this can be useful for
    stopping timers or intervals
  * _setupGpt_: initialization of GPT when _AdvertisingProvider_ component mounts
  * _teardownGpt_: GPT is deactivated when _AdvertisingProvider_ component unmounts
* *this* in the scope of the callback refers to the advertising module that is used internally to manage 
  to Prebid and GPT; *this.config* is the configuration you pass into the *AdvertisingProvider* component;
  in our example, we put the conversion rate from US$ to EUR in our config
  
---

← [[Custom Events]]
