# webpack-session

-- Start Documentation By Mufti Muntasir Ahmed 23-10-2016

```javascript
import path from 'path';
import autoprefixer from 'autoprefixer';
import webpack from 'webpack';

import CopyWebpackPlugin from 'copy-webpack-plugin';
import ExtractTextPlugin from 'extract-text-webpack-plugin';

import { config } from '../package.json';
import autoprefixerConfig from '../node-config/autoprefixer.json';

export const source = path.resolve(path.join(config.static.src, config.app));
export const destination = path.resolve(path.join(config.static.dest, config.app));

export default function() {
    return {
        entry: [
            'babel-polyfill',
            path.join(source, 'scss', 'main.scss'),
            path.join(source, 'js', 'main.js'),
        ],

        module: {
            loaders: [
                {
                    test: /\.scss$/,
                    loader: ExtractTextPlugin.extract('style', 'raw!postcss!sass')
                },

                {
                    test: /\.js$/,
                    exclude: [/node_modules/],
                    loader: 'babel'
                },

                {   test: /bootstrap\/dist\/js\/umd\//,
                    loader: 'imports?jQuery=jquery'
                }



            ]
        },

        sassLoader: config.sass,

        output: {
            filename: 'js/[name].js',
            path: destination,
        },

        postcss: [
            autoprefixer(autoprefixerConfig),
        ],

        plugins: [
            new ExtractTextPlugin('css/[name].css'),
            new CopyWebpackPlugin([
                {
                    from: path.join(source, 'assets'),
                    to: path.join(destination, 'assets')
                },
            ]),
            new webpack.NoErrorsPlugin(),
            new webpack.ProvidePlugin({
               $: "jquery",
               jQuery: "jquery",
                jquery: 'jquery',
                "window.Tether": "tether"



           })
        ]
    }
};
```


1. Load External Library into js file using Webpackk Application.

	Step - 1 :
		Let's assume you already insatalled your library ( Example: npn bootstrap ). Now we need to load to our config Js file of webpack folder.I am using bootstrap for example. Configuration would be(@config.base.js):
```javascript
	module: {
		loaders: [
					{   test: /bootstrap\/dist\/js\/umd\//,
						loader: 'imports?jQuery=jquery'
					}
				]
			}
```
After loading this js file, now we need to check our dependecy of plugin for using.Here i am using bootsrap 4.0.0 and it need Jquery plugin and windows tether.So, i am adding this plugin in to the config js file (@config.base.js):
```javascript
	new webpack.ProvidePlugin({
			   $: "jquery",
			   jQuery: "jquery",
				jquery: 'jquery',
				"window.Tether": "tether"
		   })
```
For your better understanding i am providing many of sample plugins depnedencies for future reference to use.
```
	"window.jQuery": "jquery",
	Tether: "tether",
	"window.Tether": "tether",
	Tooltip: "exports?Tooltip!bootstrap/js/dist/tooltip",
	Alert: "exports?Alert!bootstrap/js/dist/alert",
	Button: "exports?Button!bootstrap/js/dist/button",
	Carousel: "exports?Carousel!bootstrap/js/dist/carousel",
	Collapse: "exports?Collapse!bootstrap/js/dist/collapse",
	Dropdown: "exports?Dropdown!bootstrap/js/dist/dropdown",
	Modal: "exports?Modal!bootstrap/js/dist/modal",
	Popover: "exports?Popover!bootstrap/js/dist/popover",
	Scrollspy: "exports?Scrollspy!bootstrap/js/dist/scrollspy",
	Tab: "exports?Tab!bootstrap/js/dist/tab",
	Tooltip: "exports?Tooltip!bootstrap/js/dist/tooltip",
	Util: "exports?Util!bootstrap/js/dist/util"
```
	Step -2:
	Now we need to import the loader if we need to use it (@main.js). It has many ways to import. 
	These are follows:
	
```javascript
	import 'bootstrap';
	import bootstrap from 'bootstrap';
	import bootstrap from 'bootstrap/dist/js/bootstrap';
```
									
Above examples are importing your desired library. We have to careful while importing libraries , because it may cause error if it is properly not imported.
			
			
				

-- End Mufti Muntasir Ahmed
