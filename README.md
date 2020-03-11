# images
Viewer for images generated by the picroscope and uploaded to the prp

## Project setup
Install node/npm and then:
```
npm install
```
Make sure your dependencies match (e.g., 'npm i @vue/cli-plugin-babel@4.0.5' and so on)
```
npm list --depth=0
images@0.1.0 /Users/erikjung/Documents/braingeneers/images
*@vue/cli-plugin-babel@4.0.5
*@vue/cli-plugin-eslint@4.0.5
*@vue/cli-service@4.0.5
*babel-eslint@10.0.3
*core-js@3.3.6
*eslint@5.16.0
*eslint-plugin-vue@5.2.3
*hammerjs@2.0.8
*push-dir@0.4.1
*vue@2.6.10
*vue-template-compiler@2.6.10
```

### Compiles and hot-reloads for development
```
npm run serve
```
Open a browser to http://localhost:8080/images/ to view. Changes to the source will auto reload.

### Compiles and minifies and pushes to GitHub pages for production
```
npm run deploy
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### CORS
To allow objects to be access cross domains
aws --profile prp-braingeneers --endpoint https://s3.nautilus.optiputer.net s3api put-bucket-cors --bucket braingeneers --cors-configuration file://cors.json 

