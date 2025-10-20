# JS random notes

## Interview

- [@vvscode/js--interview-questions](https://github.com/vvscode/js--interview-questions)
- [telegram/@callforward](https://t.me/callforward)

## CreateReactApp with multiple entrypoints

[#webpack #eject #cra #multi #entrypoint #config]

1.  `npm run eject`
2.  add multiple entry to `webpack.config.dev.js`

```js
// /config/webpack.config.dev.js

entry: {
    index: [
      require.resolve('react-dev-utils/webpackHotDevClient'),
      require.resolve('./polyfills'),
      require.resolve('react-error-overlay'),
      paths.appIndexJs,
    ],
    admin: [
      require.resolve('react-dev-utils/webpackHotDevClient'),
      require.resolve('./polyfills'),
      require.resolve('react-error-overlay'),
      paths.appSrc + '/admin.js',                               // <-----
    ]
  },
  output: {
    path: paths.appBuild,
    pathinfo: true,
    filename: 'static/js/[name].bundle.js',                     // <-----
    chunkFilename: 'static/js/[name].chunk.js',
    publicPath: publicPath,
    devtoolModuleFilenameTemplate: info =>
      path.resolve(info.absoluteResourcePath),
  },
```

3.  modify `HtmlWebpackPlugin`

```js
// /config/webpack.config.dev.js

new HtmlWebpackPlugin({
  inject: true,
  chunks: ['index'],
  template: paths.appHtml,
  // filename:  defaults to `index.html`
}),
new HtmlWebpackPlugin({
  inject: true,
  chunks: ['admin'],
  template: paths.appHtml,
  filename: 'admin.html', // output filename
}),
```

4.  add redirect to `WebpackDevServer`

```js
// /config/webpackDevServer.config.js

historyApiFallback: {
  disableDotRule: true,
  rewrites: [
    { from: /^\/admin.html/, to: '/build/admin.html' },
  ]
}
```

> Note: repeat 2 and 3 steps with `config/webpack.config.prod.js`

## Api

- [`Intl`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl) - [#i18n #intl #native]
