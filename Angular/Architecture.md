# Angular Architecture

## Architecture Considerations

1. App Overview

2. App Features

3. Domain Security

   ​	HttpInterceptor

4. Domain Rules

5. Logging

6. Services/Communication

7. Data Models

8. Feature Components

9. Shared Functionality

10. Others

    - ​    Accessibility
    - ​	i18n
    - ​	Environments
    - ​	CI/CD
    - ​	CDN,container,Server(Docker)
    - ​	Unit testing
    - ​	End-to-end testing
    - ​	APIs

## Style guide

https://angular.io/guide/styleguide

## Organizing Features & Modules

- ### Feature Modules

  Use feature based approach to organize the modules.

  1. Customers

     customer.routing.module
     customer.module

  2. Lazyload

  3. Components declared as static in routing module

  4. Keep Flat as possible 

  5. DRY code (Do Not Repeat Yourself code)

     

- ### Core and Shared Modules

  1. Core folder should contain singleton services shared throughout app. It should be imported only once.
  
  2. Services that are specific to a feature can go in the feature's folder.
  
  3. Shared folder should contain reusable components, pipes, directives
  
  4. Reusable components across the apps should be a angular library.
  
     
  
- ### Module to load only once

  Code to load the module only once

  ```typescript
  export class EnsureModuleloadedOnceGuard { 
     constructor(targetModule: any){
        	if (targetModule) { 
               throw new Error(`${targetModule.constructor.name} has already been loaded. Import this module in the AppModule only.`); 
          	} 
        } 
  } 
  
  import { EnsureModuleloadedOnceaard }from './ensure-modulemloaded-once.guard'; 
  export class CoreModule extends EnsureModuleloadedOnceGuard { 
  	constructor(@Optional() @SkipSelf() parentModule: CoreModule) { 
          super(parentModule); 
  	} 
  } 
  
  
  ```

  

- ### Structuring Components

  **Container and Presentation Components**

  ![](C:\ravi\Training\Notes\Angular\images\route-strategy.png)

  

  ```typescript
  const routes: Route = [
      {
          path: 'custom/:id',
          component: CustomerComponent,
          children: [
              { path: 'edit', component: CustomerEditComponent },
              { path: 'details', component: CustomerDetailsComponent }
          ]
      }
  ];
  ```

  

- ### Passing state with @Input and @Output properties

  ```typescript
  // child component or directive
  @Input () customer:any; //*** 
  @Output() customerChanged = new EventEmitter<any>(); //***
  
  this.customerChanged.emit(this.customer); //***
  ```

  ```html
  <!--html or template -->
  [customer]="customer"
  (customerChanged)="changed($event)"
  ```

  ```typescript
  // parent component
  changed(customer){
  	this.customer = customer;
  }
  ```

  

- ### Change Detection Strategies

  It will not allow the modification of data at the presentation level.
  But it will allow the initialization.

  `ChangeDetectionStrategy.OnPush`

  ```typescript
  @Component({ 
      selector: 'app-customer-details',
      templateUrl: './customer-detalls.components.html',
      styleUrls: ['./customer-detalls.component.css'],
      changeDetection: ChangeDetectionStrategy.OnPush 
  })
  ```

  

- ### Cloning Techniques

  Cloning will help detect the changes to the object which will fire the onchange event

  1. `JSON.parse(JSON.stringify(object))`

  2. Npm cloning package

     ```typescript
     import { Injectable } from '@angular/core'; 
     import * as clone from 'clone'; 
     @Injectable({
         providedIn: 'root' 
     } 
     export class ClonerService {
         deepClone<T>(value) : T { 
         	return clone<T>(value); 
     	}         
     } 
     
     
     ```

     

  3. Immutable.js

- ### Component Inheritance

## Commands

- **Creating custom library**

  `ng new my-project`
  `ng gernerate library my-lib`
  `ng build my-lib`

- **Publishing a custom library**

  `ng build my-lib`
  `cd dist/my-lib`
  `npm publish`

  *Consuming the custom library is straight forward, import the shared module and use the selector in the app. Try to build the shared library before doing a ng serve.*