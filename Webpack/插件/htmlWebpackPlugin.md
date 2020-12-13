### html-webpack-plugin
```
{
  // ...
  plugins: [
    new HtmlWebpackPlugin({
      // Required
      inject: false,
      template: require('html-webpack-template'),
      // template: 'node_modules/html-webpack-template/index.ejs',

      // Optional
      appMountId: 'app',
      appMountHtmlSnippet: '<div class="app-spinner"><i class="fa fa-spinner fa-spin fa-5x" aria-hidden="true"></i></div>',
      headHtmlSnippet: '<style>div.app-spinner {position: fixed;top:50%;left:50%;}</style >',
      bodyHtmlSnippet: '<custom-element></custom-element>',
      baseHref: 'http://example.com/awesome',
      devServer: 'http://localhost:3001',
      googleAnalytics: {
        trackingId: 'UA-XXXX-XX',
        pageViewOnLoad: true
      },
      meta: [
        {
          name: 'description',
          content: 'A better default template for html-webpack-plugin.'
        }
      ],
      mobile: true,
      lang: 'en-US',
      links: [
        'https://fonts.googleapis.com/css?family=Roboto',
        {
          href: '/apple-touch-icon.png',
          rel: 'apple-touch-icon',
          sizes: '180x180'
        },
        {
          href: '/favicon-32x32.png',
          rel: 'icon',
          sizes: '32x32',
          type: 'image/png'
        }
      ],
      inlineManifestWebpackName: 'webpackManifest',
      scripts: [
        'http://example.com/somescript.js',
        {
          src: '/myModule.js',
          type: 'module'
        }
      ],
      title: 'My App',
      window: {
        env: {
          apiHost: 'http://myapi.com/api/v1'
        }
      }

      // And any other config options from html-webpack-plugin:
      // https://github.com/ampedandwired/html-webpack-plugin#configuration
    })
  ]
}
```

| Name       | Type |         Default | Description | 
| :--------- | :--: | :-----------: | :----------- |
|   title   |  {string}  |   Webpack App   |The title to use for the generated HTML document  |
| filename| {string}|'index.html' | 	The file to write the HTML to. Defaults to index.html. You can specify a subdirectory here too (eg: assets/admin.html)|
|publicPath （output自带）|	{String  'auto'}	|'auto'	|The publicPath used for script and link tags|
|favicon|	{String}|	``	|Adds the given favicon path to the output HTML|
|meta	|{Object}|	{}	|Allows to inject meta-tags. E.g. meta: {viewport: 'width=device-width, initial-scale=1, shrink-to-fit=no'}|
|hash （output自带）	|{Boolean}	|false|	If true then append a unique webpack compilation hash to all included scripts and CSS files. This is useful for cache busting


> **generate**
> 
> 动词：
> 1. 生成  `generate,  be produced`
> 2. 产生  `produce, generate, yield, bring, emerge, bring about`
> 
> 形容词：
> 1. 发生的 `generant, generate, generated, genesial, genetic`