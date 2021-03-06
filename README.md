# DC-Electron

[**ð¨ð³ ä¸­æ**](./README.md) | [**ð¬ð§English**](./README_en.md)

> è¯¥èææ¶åºäº VueCli4 ã vue-cli-plugin-electron-builder ã @dvgis/dc-sdk æ­å»ºï¼ç¨äºå¿«éæå»º 3D æ¡é¢ç«¯åºç¨ã

## å¯å¨

```node
yarn run serve
yarn run electron:serve
```

## æå

```node
yarn run build
yarn run electron:build
```

## éç½®è¯´æ

```js
const dvgis = './node_modules/@dvgis'
module.exports = {
  // å¶ä»éç½®
  chainWebpack: config => {
    config.resolve.alias.set('dvgis', path.resolve(__dirname, dvgis))
    config.plugin('copy').use(CopywebpackPlugin, [
      [
        {
          from: path.join(__dirname, 'public'),
          to: path.join(__dirname, 'dist'),
          ignore: ['index.html']
        },
        {
          from: path.join(dvgis, 'dc-sdk/dist/resources'),
          to: path.join(__dirname, 'dist', 'libs/dc-sdk/resources')
        }
      ]
    ])
  },
  pluginOptions: {
    electronBuilder: {
      chainWebpackMainProcess: config => {
        let outputDir = 'dist_electron/bundled'
        fs.removeSync(path.join(__dirname, outputDir, 'Assets'))
        fs.removeSync(path.join(__dirname, outputDir, 'Widgets'))
        fs.removeSync(path.join(__dirname, outputDir, 'Workers'))
        fs.removeSync(path.join(__dirname, outputDir, 'ThirdParty'))
        config.plugin('copy').use(CopywebpackPlugin, [
          [
            {
              from: path.join(__dirname, 'public'),
              to: path.join(__dirname, outputDir),
              ignore: ['index.html']
            },
            {
              from: path.join(dvgis, 'dc-sdk/dist/resources'),
              to: path.join(__dirname, outputDir, 'libs/dc-sdk/resources')
            }
          ]
        ])
      },
      chainWebpackRendererProcess: config => {
        config.plugin('define').tap(args => {
          const env = args[0]['process.env']
          for (let key in env) {
            args[0][`process.env.${key}`] = env[key]
          }
          delete args[0]['process.env']
          return args
        })
      }
    }
  }
}
```

## å¨å±åé Config

> è·åå¯¹åºéç½®æä»¶èµäºçå¼

> æ¡é¢ç«¯ï¼ å½åç¨æ·ç®å½ä¸ç **_.dc-conf/config.json_**

> Web ç«¯ï¼ é¡¹ç®ç®å½ä¸ç **_public/config/config.json_**

## ç¤ºä¾

![pic](https://github.com/Digital-Visual/dc-electron/blob/master/pic.png)
