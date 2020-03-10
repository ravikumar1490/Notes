# Debugging



1. **Prod Build**

   Most of the error will be displayed when we run the build with `prod` tag.

   ```bash
   ng build --prod
   ```

   

2. **Browser tools** 

   We can use editor itself to debug. We just need to set up the configurations to launch the chrome from VS code.

   Open chrome in remote debugging mode

   ```bash
   "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe"   --remote-debugging-port=9222
   ```

   `launch.json`

   ```js
   {
       "version": "0.2.0",
       "configurations": [
           {
               "type": "chrome",
               "request": "attach",
               "name": "local",
               "port": 9222,
               "urlFilter": "http://localhost:8888/*",
               "webRoot": "${workspaceFolder}"
             }
       ]
   }
   ```

   

3. **Pipes**

   We can use pipe such as `json` and `async`to print out the value in template.

   ```html
   {{data | async | json}}
   ```

   

4. **ng.probe & ng.profiler.timeChangeDetection**

   We can debug using the in-built methods of angular. Using `ng.probe` we will be be able to access the instance variables of the components. `ng.profiler.timeChangeDetection` can be called to see the changes immediately after making changes using `ng.probe` .

   ```js
   let title = $('div > h2');
   ng.probe(title).componentInstance.title = 'Hi, Im ravi'; // changes the text but will not be able to see the change in browser.
   ng.profiler.timeChangeDetection(); // Now the changes are detected and changes will be displayed.
   
   ```

   *The things what `ng.probe` does can be achieved using augury chrome plugin*

   

5. **Chrome plugins**

   1. augury - Modify the instance variable of components. Call the instance methods.
   2. redux - We will able to debug the state management. It has nice way to play back and forward.

