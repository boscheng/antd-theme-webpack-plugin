# antd-theme-webpack-plugin

This webpack plugin is to  generate color specific less/css and inject into your `index.html` file so 
that you can change Ant Design specific color theme in browser.

## Live Theme Demo: 

https://mzohaibqc.github.io/antd-theme-webpack-plugin/index.html

https://antd-live-theme.firebaseapp.com/

In order to integrate with your webpack configurations, install the package and add following code in your webpack config file.

## Install
  - npm install -D antd-theme-webpack-plugin

```js
const AntDesignThemePlugin = require('antd-theme-webpack-plugin');

const options = {
  antDir: path.join(__dirname, './node_modules/antd'),
  stylesDir: path.join(__dirname, './src'),
  varFile: path.join(__dirname, './src/styles/variables.less'),
  themeVariables: ['@primary-color'],
  indexFileName: 'index.html',
  generateOnce: false,
  lessUrl: "https://cdnjs.cloudflare.com/ajax/libs/less.js/2.7.2/less.min.js",
  publicPath: "",
  customColorRegexArray: [], // An array of regex codes to match your custom color variable values so that code can identify that it's a valid color. Make sure your regex does not adds false positives.
}

const themePlugin = new AntDesignThemePlugin(options);
// in config object
plugins: [
    themePlugin
  ]
```
Add this plugin in `plugins` array.


| Property | Type | Default | Descript |
| --- | --- | --- | --- |
| antDir | string | - | This is path to antd directory in your node_modules |
| stylesDir | string | - | This is path to your custom styles root directory, all files with .less extension in this folder and nested folders will be processed  |
| varFile | string | - | Path to your theme related variables file |
| themeVariables | array | ['@primary-color'] | List of variables that you want to dynamically change |
| indexFileName | string | index.html | File name of your main html file, in most cases it is `index.html` which is default |
| lessUrl | string | https://cdnjs.cloudflare.com/ajax/libs/less.js/2.7.2/less.min.js | less.js cdn or file path |
| publicPath | string | '' | This string will be prepended to `/color.less` in `index.html` file in case |
| generateOnce | boolean | false | Everytime webpack will build new code due to some code changes in development, this plugin will run again unless you specify this flag as `true` which will just compile your styles once |
| customColorRegexArray | array | ['color', 'lighten', 'darken', 'saturate', 'desaturate', 'fadein', 'fadeout', 'fade', 'spin', 'mix', 'hsv', 'tint', 'shade', 'greyscale', 'multiply', 'contrast', 'screen', 'overlay'].map(name => new RegExp(`${name}\(.*\)`))] | This array is to provide regex which will match your color value, most of the time you don't need this |


```

If you `index.html` file is not being generated by build process then add following code in your `index.html` or whatever is the name of html main file and add `indexFileName: false` in options/config. This way you can better place your below script in your html file according to your needs.

```
<link rel="stylesheet/less" type="text/css" href="/color.less" />
<script>
  window.less = {
    async: false,
    env: 'production'
  };
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/less.js/2.7.2/less.min.js"></script>
```
Don't forget to add import Ant design default theme file i.e. antd/lib/style/themes/default.less in variables.less file.

# Enable Javascript for less-loader

You need to enable javascript for less-loader.

```
{
  javascriptEnabled: true
}

```

For those who are using `react-app-rewire-less` with `react-app-rewired`, enable javascript like this

```
module.exports = function override(config, env) {
  config = injectBabelPlugin(['import', { libraryName: 'antd', style: true }], config);
  config = rewireLess.withLoaderOptions({
    modifyVars: {
      '@primary-color': '#002251'
    },
    javascriptEnabled: true
  })(config, env);
  config.plugins.push(new AntDesignThemePlugin(options));
  return config;
};
```

## Configurations using customize-cra
https://github.com/mzohaibqc/antd-theme-webpack-plugin/blob/master/examples/customize-cra/config-overrides.js

## Light/Dark Theme Switch
Here is a demo to switch between light and dark themes dynamically.
https://mzohaibqc.github.io/antd-theme-webpack-plugin/index.html

And here is code for this demo
https://github.com/mzohaibqc/antd-theme-webpack-plugin/tree/master/examples/customize-cra

Note: you don't necessarily

## [CHANGELOG](/CHNAGELOG.md)
