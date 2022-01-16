# Fancy Gatus

Fancy Gatus is an alternative frontend for the monitoring tool [Gatus](https://github.com/TwiN/gatus). The goal is to provide a simplified and modern status page based on data from a Gatus instance, which only displays the most important information in a way that is understandable for end users.

You can see it in action here: https://status.bluemedia.dev

![Demo screenshot](docs/demo-screenshot.png)

## Configuration

The frontend tries to retrieve a configuration file named `config.json` from the webroot during page load. If the configuration is loaded successfully, it will be used to make advanced adjustments to the frontend. The possible options are listed below.

| Parameter         | Description                                                                                                                                                                                                                                                              | Default                 |
|:------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------|
| `title`           | Title of the page. Both in the tab and next to the logo.                                                                                                                                                                                                                 | `Infrastructure Status` |
| `gatusBaseUrl`    | Alternative base URL (without trailing slash) of the Gatus instance, if the API is not available relative to the frontend.                                                                                                                                               | `""`                    |
| `hiddenGroups`    | Array containing names of groups that should be hidden. These groups are still visible in the API response!                                                                                                                                                              | `[]`                    |
| `hiddenEndpoints` | Array containing names of endpoints that should be hidden. These endpoints are still visible in the API response!                                                                                                                                                        | `[]`                    |
| `groupOrder`      | Array containing names of groups. The groups are sorted in the frontend according to the order in the array (different from alphabetical sorting by default). If groups are not included in the array, they will be added alphabetically sorted below the sorted groups. | `[]`                    |

## Deployment

Fancy Gatus is intended to be delivered directly from a web server (e.g. Nginx). The installation is therefore simple.  
Just download the latest ZIP file from the release page and unpack it into the web root of your server. If you want to make further adjustments to the frontend, you can also create the configuration file in the web root. Refer to the [Configuration](#configuration) section for more information.

Make sure that the Gatus API endpoint `/api/v1/endpoints/statuses` is available relative to the frontend if you have not configured a different base URL. An example configuration for Nginx that makes this possible can be found [here](docs/example-nginx.conf).

### Customizing the logo

If you want to use your own logo, you can simply replace the file in the `img` folder. The image should be square, have a minimum size of 250x250 pixels and preferably a transparent background.  
  
You can also easily replace the favicon. You can find various converters for the `ico` format online.

## Building Fancy Gatus

You can also build the project yourself instead of using one of the pre built releases. To do this, you need the following requirements:

- `NodeJS >= v14.16.0`
- `Yarn >= v1.22.17`

If you have the necessary requirements, you can use `yarn install` to install the dependencies and `yarn build` to run the build process. The finished data for your web server can be found in the `dist` folder.

## Development

Fancy Gatus uses Vue.js 3 as its frontend framework. You can therefore use all common Vue.js commands:

Compiles and hot-reloads for development
```
yarn serve
```

Compiles and minifies for production
```
yarn build
```

Lints and fixes files
```
yarn lint
```
### Contribution Guidelines

- Use 2 spaces indent and camelCase
- Comment code (in english), if it has a certain complexity
- Stick to the structure
- Test your changes
- Update the documentation
