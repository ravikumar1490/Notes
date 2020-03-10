# Angular Fundamentals



**Bootstrapping an Angular App**

`angular.json`

```js
"build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            ...
            "index": "src/index.html", // index file is referred here
            "main": "src/main.ts", // main file is referred here
           	...
          }
    }
```



`main.ts`

```js
platformBrowserDynamic().bootstrapModule(AppModule) // App module is bootstrapped here
  .catch(err => console.error(err));
```



`app.module.ts`

```typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent] // App component is bootstrapped here
})
export class AppModule { }
```



`index.html`

```html
<app-root> <app-root> <!-- App component selector -->
```



**Referencing static files**

`angular.json`

```js
"build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            ...
            "assets": [     //assets are refrenced here
              "src/favicon.ico",
              "src/assets"
            ],
            "styles": [     //styles are referenced here
              "src/styles.css"
            ],
            "scripts": [    //scripts are referenced here
              "node_modules/jquery/dist/jquery.min.js",
              "node_modules/bootstrap/dist/js/bootstrap.js"
            ]   
          }
}
```

