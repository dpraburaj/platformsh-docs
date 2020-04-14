# Headless Chrome

Headless Chrome is a headless browser that can be configured on projects like any other service on Platform.sh. You can interact with the `headless-chrome` service container using Puppeteer, a Node library that provides an API to control Chrome over the DevTools Protocol.

Puppeteer can be used to generate PDFs and screenshots of web pages, automate form submission, and test your project's UI. You can find out more information about using Puppeteer on [GitHub](https://github.com/GoogleChrome/puppeteer) or in their [documentation](https://pptr.dev/).

## Supported versions

* 73

## Relationship

The format exposed in the `$PLATFORM_RELATIONSHIPS` [environment variable](/development/variables.md#platformsh-provided-variables):

```yaml
{
    "service": "headless",
    "ip": "169.254.73.96",
    "hostname": "3rxha4e2w4yv36lqlypy7qlkza.headless.service._.eu-3.platformsh.site",
    "cluster": "moqwtrvgc63mo-master-7rqtwti",
    "host": "headless.internal",
    "rel": "http",
    "scheme": "http",
    "type": "chrome-headless:73",
    "port": 9222
}

```

## Requirements

Puppeteer requires at least Node.js version 6.4.0, while using the async and await examples below requires Node 7.6.0 or greater.

Using the Platform.sh [Config Reader](https://github.com/platformsh/config-reader-nodejs) library requires Node.js 10 or later.

### Other languages

It will be necessary to upgrade the version of Node.js in other language containers before using Puppeteer. You can use [Node Version Manager](https://github.com/nvm-sh/nvm) or NVM to change or update the version available in your application container by following the instructions in the [Alternate Node.js install](/languages/nodejs/nvm.html) documentation.

## Usage example

In your `.platform/services.yaml`:

{% codesnippet "/registry/images/examples/full/chrome-headless.services.yaml", language="yaml" %}{% endcodesnippet %}

In your `.platform.app.yaml`:

{% codesnippet "/registry/images/examples/full/chrome-headless.app.yaml", language="yaml" %}{% endcodesnippet %}

After configuration, include Puppeteer as a dependency in your `package.json`:

```json
"puppeteer": "^1.14.0"
```

Using the [Node.js Config Reader](https://github.com/platformsh/config-reader-nodejs) library, you can retrieve formatted credentials for connecting to headless Chrome with Puppeteer:

```
const platformsh = require('platformsh-config');

let config = platformsh.config();
const credentials = config.credentials('chromeheadlessbrowser');
```

and use them to define the `browserURL` parameter of `puppeteer.connect()` within an `async` function:

```
exports.takeScreenshot = async function (url) {
    try {
        // Connect to chrome-headless using pre-formatted puppeteer credentials
        const formattedURL = config.formattedCredentials('chromeheadlessbrowser', 'puppeteer');
        const browser = await puppeteer.connect({browserURL: formattedURL});

        ...

        return browser

    } catch (e) {
        return Promise.reject(e);
    }
};
```

Puppeteer allows your application to [create screenshots](https://pptr.dev/#?product=Puppeteer&version=v1.17.0&show=api-pagescreenshotoptions), [emulate a mobile device](https://pptr.dev/#?product=Puppeteer&version=v1.17.0&show=api-pageemulateoptions), [generate PDFs](https://pptr.dev/#?product=Puppeteer&version=v1.17.0&show=api-pagepdfoptions), and much more.

You can find some useful examples of using headless Chrome and Puppeteer on Platform.sh on the Community Portal:

* [How to take screenshots using Puppeteer and Headless Chrome](https://community.platform.sh/t/how-to-take-screenshots-using-puppeteer-and-headless-chrome/305)
* [How to generate PDFs using Puppeteer and Headless Chrome](https://community.platform.sh/t/how-to-generate-pdfs-using-puppeteer-and-headless-chrome/306)