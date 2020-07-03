By default, the *react-advertising* bundle does not include any polyfills to enable compatibility with older browsers. This is done intentionally to keep the bundle size small.

The downside of this is that *react-advertising* is by default **not** compatible with Microsoft Internet Explorer 11, since IE11 is not in active development anymore, hasn't been for quite some time, hence does not support many of the features of modern browsers.

To remedy this, you can use [babel-polyfill](https://babeljs.io/docs/en/babel-polyfill) to emulate modern browser features on older browsers. There's a good chance your web app is already using this, it's quite common.

The easiest way to include *babel-polyfill* in your web app is by adding a script tag to the *head* section of your HTML code.

**When using Babel 7 (most recent):**

    <script nomodule src="https://cdnjs.cloudflare.com/ajax/libs/babel-polyfill/7.4.4/polyfill.min.js" integrity="sha256-lu1gm0Fb5u5n6tuNLefOZNE96ckovOjhNzvsl+Iz50w=" crossorigin="anonymous"></script>

**When using Babel 6:**

    <script nomodule src="https://cdnjs.cloudflare.com/ajax/libs/babel-polyfill/6.26.0/polyfill.min.js" integrity="sha256-WRc/eG3R84AverJv0zmqxAmdwQxstUpqkiE+avJ3WSo=" crossorigin="anonymous"></script>

Note the *nomodule* attribute – this makes sure the additional bundle is only loaded when needed, i.e. if the browser does not support ES6 modules.

Modern browsers like Firefox, Chrome, Edge, Opera or Safari don't need the polyfill, so the client is not burdened with downloading the additional library.

Versions of Internet Explorer older than IE11 are **not** supported.

---

← [[Usage]] | [[Advanced Usage]] →