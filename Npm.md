# NPM

We can handle build scripts from package.json itself. 

- Lightweigt
- Run Sass conversion
- Transpiling ES 6 components

## Special scripts

https://docs.npmjs.com/misc/scripts
Example: start ,stop etc

## Commands

- **Installation**

  `npm install --save package-name`
  Packages will get added to dependencies.

  `npm install --save-dev package-name`
  Packages will get added to dependencies.

- **Run**

  `npm run command-from-script`

  ```js
  scripts:{
  	"dev:serve": "live-server build/development",
  	"start" : "npm run  dev:serve",
  	"prod-folder": "mkdirp build/production",
  	"prestart": "npm run prod-folder "
  }
  ```

- **Special script**

  `npm start`

