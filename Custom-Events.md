The ads you display on your page are confined to the provided slots, usually inside an
[iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) element.

Sometimes, however, it is desirable to be able to control the layout of the whole page for advertising. A common use
case, for example, is setting the page background color to match that of an ad creative.

This can be done by dispatching a message from the iframe delivered by the ad server to the parent page using the
browser's [postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) API, as
[described in this blog post](https://davidwalsh.name/window-iframe).

_React-advertising_ allows use to set up these custom events in your [[config|Configuration#custom-events]] and then pass a
callback function to the [[AdvertisingSlot|API#advertisingslot]] component as _customEventHandlers_ prop.

Let's configure a custom event for changing the background color, for example, by adding this to the ad config:

```javascript
const config = {
  slots: [
    {
      id: "my-ad-id"
    }
  ],
  customEvents: {
    blueBackground: {
      eventMessagePrefix: "BlueBackground:"
    }
  }
};
```

We then pass this config to an [[AdvertisingProvider|API#advertisingprovider]] component and pass the callback to change
the page background to an ad slot:

```javascript
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      backgroundColor: "red"
    };
  }

  render() {
    return (
      <AdvertisingProvider config={config}>
        <div style={{ backgroundColor: this.state.backgroundColor }}>
          <AdvertisingSlot
            id="my-ad-id"
            customEvents={{
              blueBackground: () => this.setState({ backgroundColor: "blue" })
            }}
          />
        </div>
      </AdvertisingProvider>
    );
  }
}
```

Note that you don't pass the callback function that sets the background color directly to the _AdvertisingSlot_, but
rather as an object, where the key corresponds to the key of the custom event in the config (i.e. _blueBackground_).

You can even pass more than just one callbacks if need be.

With the setup above in place, you can then add a script snippet to the Google tag code delivered by DFP that changes
the background color when the ad is served:

```html
<script>parent.postMessage('BlueBackground:my-ad-id', '*');</script>
```

* The first parameter for _postMessage_ consists of
  * the custom event message prefix you have configured
  * the ID of the ad
* The second parameter is the target origin, an asterix as in the example above should be sufficient in most cases
  (refer to [postMessage documentation](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) for
  details)

When the ad is now delivered from the ad server, the background color of the wrapper div in the *App* component
turns from red to blue.

---

← [[Configuration]] | [[Plugins]] →
