# Tardisproxy Dev Docs

## How to use? + Bundling

TardisProxy will simply be a WebPack plug-in that will require the path to your main server code file as an argument and optionally a config. It will then pack this into the internal routing system that it uses. There will be seperate bundles built for each type of injection method: bookmarklet and script. This will also allow for more powerful domain fronting.

### Here is the config

```ts
interface Config {
  serverRuntime: "Bun" | "Deno" | "Node";
  bareServers: Array<string>;
  updateEndpoints: Array<string>;
}
```

## Server-code

The JS runtime (Node/Deno/Bun) server code will be ran through my [SW subsystem projects](https://hedge.soundar.eu.org/s/3XYGm0DQx) and then that SW will then be ran through my [SW-less runtime for proxies](https://github.com/VyperGroup/SW-less).

## Default Frontend (No proxy site applied)

This will put a fake draggable window that simply hosts the proxy site in an sandboxed iframe. Any redirect will be intercepted and handled by the injected scripts in the parent. It will then trigger a HTTP request and expect a response from the web server registered in the JS Runtime itself. When it gets this response it will simply replace the iframe with whatever is in the response.

### Window decorations

#### On the left

- settings - This will let you configure bare servers to be used

#### On the right

- toggle - Instead of having minimize
- close - This will cleanup the code and close the window

##### On the bottom right

- resize corner - ...

## Proxy Backend

It will try these backends

1. When you run the script on Google Translate, it will actually use Google Translate as the backend! The website translation feature is [actually a web proxy in of itself, and the actual http response is mostly unmodified](view-source:https://example-com.translate.goog/?_x_tr_sl=auto&_x_tr_tl=en&_x_tr_hl=en). This functionality is unique to this project.
2. My GAS Bare Servers, which would break Google Drive itself, if you try to block it.
3. Built-in servers

If none of these work, the user will be asked to provide their own bare servers. They will be able to customize this anyways.

## Autoupdating

The API will notify of and distribute new updates. demonstrate how too update when applicable.

```bash
$VERSION = $API_URL/currentVersion
$MANIFEST = $API_URL/get/$VERSION
```

## Ways to use (provided with examples of site injection)

- Data protocol

```html
data:text/html,
<script>
  (async () => {
    document.write(
      await (await fetch("https://enable-cors.org/server.html")).text()
    );
  })();
</script>
```

- Javascript protocol

```js
javascript: (async () => {
  document.write(
    await (await fetch("https://enable-cors.org/server.html")).text()
  );
})();
```

- Adblock Plus

...

- Domain fronting methods

  - SVGs
  - XML files with XSLT transforms
    - PDF's with [XFA](https://en.wikipedia.org/wiki/XFA) - This feature is coming to all browsers soon, and you can enable it in chrome://flags.
