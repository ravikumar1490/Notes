# React



## Quick Start

```powershell
#npx
npx create-react-app my-app
cd my-app
npm start

#npm
npm init react-app my-app

#yarn
yarn create react-app my-app
```



## Css Styling 

1. Radium
2. Styled components
3. Css Modules (You can use this after ejecting, in newer versions we can use with **<css_file_name>.module.css**)

## Life cycle hooks

​	![](C:\ravi\Training\Notes\React\images\all_lifecycle_hooks.PNG)





1. ### Creation sequence

   ![](C:\ravi\Training\Notes\React\images\creation_lifecycle.PNG)

   

2. ### Update sequence

   ![](C:\ravi\Training\Notes\React\images\update_lifecycle.PNG)

   



## React hooks

1. useState
2. useEffect
3. useRef
4. useContext

`NEEDS TO BE UPDATED`

## How React Updates The Dom

![](C:\ravi\Training\Notes\React\images\updates_dom.PNG)



## Performance Optimization

1. shouldComponentUpdate

   **Class based**, we can check for the required props changes and enable/disable re-rendering based on that.

   ```js
   shouldComponentUpdate(nextProps, nextState) {
   	if(nextProps.persons !== this.props.persons) {
           return true;
       }
       return false;
   }
   ```

   

2. React.memo

   **Function based**, re-render is called only when there is a change in the props otherwise it returns the same.

   ```js
   export default React.memo(cockpit);
   ```

   

3. PureComponent

   **Class based**, if we want to check all the props changes, then it is better to extend from PureComponent instead of Component. PureComponent will take care by itself.

   ```js
   class Persons extends PureComponent {
       
   }
   ```



## Rendering Adjacent JSX element

React doesn't allow rendering adjacent elements, it always need a root element because js requires only one element to be returned.

**problem**:

```react
render(){
    return (
        <p></p>
        <p></p>
        <p></p>
    )
}
```

**solutions:**

1. Wrapping with Root element

   ```react
   return (
       	<div>
           	<p></p>
               <p></p>
               <p></p>
       	</div>
       )
   ```
   

   
2. Passing it as array with keys
	```react
   return [
           <p key="id1"></p>,
           <p key="id2"></p>,
           <p key="id2"></p>, 
       ]
   ```

3. Creating Auxillary componetns which returns only childerns
	

It is a Higher order component(HOC), it just wraps another component and the may be adds some extra logic to it. Usually it is kept under hoc directory.

	```react
	//	Auxillary.js
	const aux = props => props.children;
	
	export default aux;
	```
	
	```react
	import Aux from './hoc/Auxillary'   
	   return (
	       	<Aux>
	           <p></p>
	           <p></p>
	           <p></p>
	       	</Aux>
	       )
	```


​	
​	
4. React Fragment

  Introduced in React 16.1, it is same as Auxillary which is provided in React library itself.

  ```react
  import React, {Component} from 'react';   
     return (
         	<React.Fragment>
             <p></p>
             <p></p>
             <p></p>
         	</React.Fragment>
         )
  ```

  ```react
  import React, {Component, Fragment} from 'react'; 
     return (
         	<Fragment>
             <p></p>
             <p></p>
             <p></p>
         	</Fragment>
         )
  ```

  

## Higher Order Components

HOC just wraps another component and the may be adds some extra logic to it. Usually it is kept under hoc directory. It can be used in multiple ways, for example it can be used for error handling, styling, addition HTML structure around the structure.

We can create HOC in 2 ways;

1. To be used when HOC deals with JSX.

```react
// WithClass.js
import React from 'react';

const withClass = props => (
	<div className={props.classes}>{props.children}</div>
);
                                    
//App.js
import React from 'react';
import WithClass from '../hoc/WithClass.js';
return (
	<WithClass classes={classes.App}>
    	<button></button>
        <Cockpit></Cockpit>
    </WithClass>
);  
export default App;
```



2. To be used when HOC doesn't deal with JSX, instead it uses it for javascript logic.

```react
// withClass.js /***/
import React from 'react';

const withClass = (WrappedComponent, className) => { /***/
    return props => (
		<div className={className}><WrappedComponent/></div> /***/
	);
};

//App.js
import React from 'react';
import withClass from '../hoc/withClass.js'; /***/
import Aux from '../hoc/Aux.js';
return (
	<Aux>
    	<button></button>
        <Cockpit></Cockpit>
    </Aux>
);  
export default withClass(App, classes.App); /***/

```

Passing unknown props in HOC.

```react
const withClass = (WrappedComponent, className) => { /***/
    return props => (
		<div className={className} ><WrappedComponent {...props} /></div> /***/
	);
};
```



## State updates

```react
// Works fine when it is not dependent on previous state.
//Coz React updates the state asynchronously
this.state({
    persons: persons
});

//Use the below one to update state when there is value depends on the prev state.
this.setState((prevState, props) => { /***/
    return {
    	persons: persons,
        changeCounter: prevState.changeCounter + 1  /***/
    }
});
```



## Prop Types

If you want to restrict the types on props, then we can use a library `prop-types`. This will be useful when we want to ship the component as library or when working with large team

Installation

> npm install --save prop-types



Usage

```react
import PropTypes from 'prop-types';

class Person extends Component {

}

Person.propTypes = {
	click: PropTypes.func,
	name: PropTypes.string,
	age: PropTypes.number,
	changed: PropTypes.func
}

export default Person;
```



## Ref

React provides a easier option to access the HTML elements so we can access those elements to focus or do such things.

```react
componentDidMount() {
    this.inputElement.focus(); /***/
}

render() {
    return (
        <input
            key="i3"
            ref ={(inputEl) => {this.inputElement = inputEl}} /***/
            type= "text"
            onChange = {this.props.changed}
            value={this.props.name}
        />
    );
}


```



After React 16.3, it provides a new way to access refs `React.createRef()`.

```react
class Person extends Component {
    constructor(props) {
        super(props);
        this.inputElementRef = React.createRef(); /***/ 
    }
    
    componentDidMount() {
        this.inputElementRef.current.focus(); /***/
    }

    render() {
        return (
            <input
                key="i3"
                ref ={this.inputElementRef} /***/
                type= "text"
                onChange = {this.props.changed}
                value={this.props.name}
            />
        );
    }
}
```



In functional component, we can make use of `useRef` hook to get the ref. We can also pass value in `useRef` to initialize.

```react
import React, {useEffect, useRef} from 'react';

const cockpit = props => {
    const toggleBtnRef = useRef(null); /***/
    
    useEffect(() => {
       toggleBtnRef.current.click(); 
    }, []);
    
    return (
    	<button ref={toggleBtnRef}> </button> /***/
    )
}

```



## Context

The context API is all about managing the data across components without the need to pass data around with props. This removes the need for passing details in hierarchy.

We can restrict which component can access the context by wrapping the context around it.

`Context.Provider` is used to pass the context and `Context.Consumer` is used to consume the context. It can be used only in JSX.

```react
//auth-context.js
import React from 'react';

const authContext = React.createContext({
	authenticated: false,
    login: () => {}
});

export default authContext;
```



```react
//App.js
import AuthContext from '../context/auth-context';

return (
    <Aux>
        <button></button>
        <AuthContext.Provider  /*** AuthContext can be used n Cockpit and Persons component*/
            value={{authenticated: this.state.authenticated, login: this.loginHandler}}>	
        	<Cockpit></Cockpit>  
            <Persons></Persons>
        </AuthContext.Provider>
    </Aux>
)


//Cockpit.js
import AuthContext from '../../context/auth-context';
const cockpit = (props) => {
    return (
        <AuthContext.Consumer> /***/
        {(context) => <button onClick={context.login}>Log in</button>}
        </AuthContext.Consumer>
    );
}
```

In React 16.6, we can consume context in more simpler way in class components. It can be used in js code as well.

```react
import AuthContext from '../../context/auth-context'; 
class Person extends Component {
    static contextType = AuthContext; //*** it needs to be static /
	componentDidMount() {
        console.log(this.context.authenticated); //***/
    }
 }
```

In functional component we can make use of `useContext` hook to get the access of the context.

```react
import AuthContext from '../../context/auth-context';
const cockpit = (props) => {
	const authContext = useContext(AuthContext);
    return (
    	<button onClick={authContext.login}>Log in</button>
    );
}
```



## Network Requests

We make use of [**Axios**](https://www.npmjs.com/package/axios) library for making network calls. It provides support for all kind of Http request.

### HTTP request

- GET

- POST

- DELETE

- PUT

  ```react
  import axios from 'axios';
  
  axios.get("/posts").then((response) => {});
  axios.post("/posts", data).then((response) => {});
  axios.delete("/posts/1").then((response) => {});
  axios.put("/posts/1", data).then((response) => {});
  ```

  

### Interceptors

We can create interceptor so we can intercept the network calls. We can do this for request and response. This is mostly done for editing the request to have `auth token` & `common headers`. Also one more use case is to handle the errors globaly.

```react
//index.js
axios.interceptors.request.use(request => {
    console.log(request);
    return request;
}, error => {
    return Promise.reject(error);
});

axios.interceptors.response.use(response => {
    console.log(response);
    return response;
}, error => {
    return Promise.reject(error);
});
```



### Default  Configuration

Axios provides easier way to set up default configuration which can passed along with each requests.

```react
//index.js
import axios from 'axios';
axios.defaults.baseURL = 'https://jsonplaceholder.typicode.com';
axios.defaults.headers.common['Authorization'] = "Auth Token";
axios.defaults.headers.post['Content-Type'] = "application/json";
```



### Instance Configuration

We can also make use of Instance configuration where we want to change configuration for some of the network calls. It overrides the default configuration. We can create as many of instance of axios and use it in the project. We can also create interceptors for the instances.

```react
//local => axios.js
import axios from 'axios';

const instance = axios.create({
    baseURL: 'https://google.com'
});

instance.defaults.headers.common["Authorization"] = 'Auth Token from Instance';

//We can also setup interceptors on instances
//instance.interceptors.request.use

export default instance;



//Blog.js
import axios from '../../axios';
componentDidMount() {
    axios.get("/posts").then((response) => {
      const posts = response.data.slice(0, 4);
      const updatedPosts = posts.map((post) => {
        return {
          ...post,
          author: "Max",
        };
      });
      this.setState({ posts: updatedPosts });
    });
  }
```





## Routing

React by default doesn't provide the routing capability. We need to install libraries `react-router` & `react-router-dom` to support routing in react app. We installed both `react-router` and `react-router-dom` . Technically, only `react-router-dom` is required for web development**. It wraps `react-router` and therefore uses it as a dependency. 

https://reactrouter.com/web/guides/philosophy

```react
// App.js
import { BrowserRouter } from "react-router-dom"; /***/

class App extends Component {
  render() {
    return (
      <BrowserRouter> 
        <div className="App">
          <Blog />
        </div>
      </BrowserRouter>
    );
  }
}
```

We need to wrap with `<BrowserRouter>` tag where we want to specify router configuration. By default `basename` property always maps to **root** **directory**. So even if you don't provide it will map to root directory. If you want to load the react application under some folder(mostly in production), then you will need to set the `basename`  property otherwise routing will not work. This works as **basePath**.

```react
<BrowserRouter basename="/"></BrowserRouter> //***
<BrowserRouter basename="/my-app"></BrowserRouter>//***
```



```react
//Blog.js
import {Route} from "react-router-dom"; /***/

 render() {
    return (
      <div className="Blog">
        <header>
          <nav>
            <ul>
              <li><a href="/">Home</a></li>
              <li><a href="/new-post">New Post</a></li>
            </ul>
          </nav>
        </header>
        <Posts></Posts>
        <Route path="/" exact render={() => <h1>Home 1</h1>} /> <!--/***/ --> <!--exact property -->
        <Route path="/" render={() => <h1>Home 2</h1>} /> <!--It will be rendered with all the routes -->
        <Route path="/" exact component={Posts} /> <!-- *** -->
        <Route path="/new-post" exact component={NewPost} />
        <Route path="/:id" exact component={FullPost} /> <!-- Dynamic parameter -->
      </div>
    );
  }
```

`exact` property on **Route** checks the complete path, otherwise it will check if it starts with path. We can have multiple Route matching the same path, all will be rendered.

`component` property where we can give the component name which we want to render if the path matches.

### Link

If we don't want to re-load the page each click, we should use `Link` component provided by react-router-dom.

```react
import {Route, Link} from "react-router-dom"; /***/

render() {
    return (
      <div className="Blog">
        <header>
          <nav>
            <ul>
              <li><Link to="/">Home</Link></li> <!--***-->
              <li><Link to={{
                  pathname: '/new-post',
                  hash:'#submit1', //hash
                  search: '?quick-submit=true'//passing query parameters
              }}>New Post</Link></li>
            </ul>
          </nav>
        </header>
      </div>
    );
  }
```

You learned about `Link` , you learned about the `to` property it uses.

The path you can use in to can be either **absolute** or **relative**. 

#### **Absolute Paths**

By default, if you just enter `to="/some-path"` or `to="some-path"` , that's an **absolute path**. 

**Absolute path** means that it's **always appended right after your domain**. Therefore, both syntaxes (with and without leading slash) lead to `example.com/some-path` .

#### **Relative Paths**

Sometimes, you might want to create a relative path instead. This is especially useful, if your component is already loaded given a specific path (e.g. `posts` ) and you then want to append something to that existing path (so that you, for example, get `/posts/new` ).

If you're on a component loaded via `/posts` , `to="new"` would lead to `example.com/new` , **NOT** `example.com/posts/new` . 

To change this behavior, you have to find out which path you're on and add the new fragment to that existing path. You can do that with the `url` property of `props.match` :

`` will lead to `example.com/posts/new` when placing this link in a component loaded on `/posts` . If you'd use the same `` in a component loaded via `/all-posts` , the link would point to `/all-posts/new` .

**There's no better or worse way of creating Link paths** - choose the one you need. Sometimes, you want to ensure that you always load the same path, no matter on which path you already are => Use absolute paths in this scenario.

Use relative paths if you want to navigate relative to your existing path.

*** Remember to check out `props`, react-router-dom adds some required properties to props. Like path, url, search(queryParams), hash and etc.



### NavLink

We can use **NavLink** for styling the active link. By default it adds the `active` class to the if the path matches with link.

By providing `exact` it looks for complete path match. We can provide custom active class by passing value to `activeClassName` attribute. We can also pass custom style to `activeStyle`

 

```react
import { Route, NavLink } from "react-router-dom";

return (
    <nav>
        <ul>
            <li>
                <NavLink to="/" exact activeClassName="my-active" activeStyle={{textDecoration: 					underline}}>Home</NavLink>
            </li>
            <li>
                <NavLink exact to="/new-post">New Post</NavLink>
            </li>
        </ul>
    </nav>
)

```



### Route Related Props

With Routing, the props gets updated with additional information related to:

1. history
2. location
3. match

The route related props are not passed to child components in tree. However, it can be passed to child components in a couple of ways. So it can be accessed from child components(child components will be route aware).

1. Passing it as props

   ```react
   <Post {...props}/>
   (or)
   //pass required props
   <Post match = {this.props.match} />
   
   ```

   

2. Using **withRouter** hoc.

   ```react
   //Post.js
   //Consuming component
   import {withRouter} from 'react-router-dom';
   
   export default withRouter(Post);
   ```



- ####  **Path Params**

  ```react
  //Blog.js
  <Route path="/:id" exact component={FullPost} />
  
  //Posts.js
  <Link to="/"+ post.id><Post></Post></Link>
  
  //Fullpost.js
  componentDidMount() {
  	console.log(this.props.match.params.id); //***/
  }
  ```

  

- #### **Query Params**

  You can pass them easily like this:

  ```react
  <Link to="/my-path?start=5">Go to Start</Link> 
  ```

  or

  ```react
  <Link 
      to={{
          pathname: '/my-path',
          search: '?start=5'
      }}
      >Go to Start</Link>
  ```

  React router makes it easy to get access to the search string: `props.location.search` .

  But that will only give you something like `?start=5` 

  You probably want to get the key-value pair, without the `?` and the `=` . Here's a snippet which allows you to easily extract that information:

  ```react
  componentDidMount() {
      const query = new URLSearchParams(this.props.location.search);
      for (let param of query.entries()) {
          console.log(param); // yields ['start', '5']
      }
  }
  ```

  `URLSearchParams` is a built-in object, shipping with vanilla JavaScript. It returns an object, which exposes the `entries()` method. `entries()` returns an Iterator - basically a construct which can be used in a `for...of...` loop (as shown above).

  When looping through `query.entries()` , you get **arrays** where the first element is the **key name** (e.g. `start` ) and the second element is the assigned **value** (e.g. `5` ).

  

- #### **Fragment**

  You can pass it easily like this:

  ```react
  <Link to="/my-path#start-position">Go to Start</Link>
  ```

  or

  ```react
  <Link 
      to={{
          pathname: '/my-path',
          hash: 'start-position'
      }}
      >Go to Start</Link>
  ```

  React router makes it easy to extract the fragment. You can simply access `props.location.hash` .	



### Switch

We might get into a problem where route paths are matching, so both components will be resolved. We can make use of Switch to load the first encountered one.

Problem:

```react
 <Route path="/new-post" exact component={NewPost} />
 <Route path="/:id" exact component={FullPost} />
```

Here, both route is treated as same, `new-post` is treated as `id` so both components will be rendered when `/new-post` is accessed.

Solution:

```react
import {Switch} from 'react-router-dom';


<Switch>
    <Route path="/" exact component={Posts} />
	<Route path="/new-post" exact component={NewPost} />
 	<Route path="/:id" exact component={FullPost} />
</Switch>

//or
<Route path="/" exact component={Posts} />
<Switch>
	<Route path="/new-post" exact component={NewPost} />
 	<Route path="/:id" exact component={FullPost} />
</Switch>
```

 This will load the first encountered one.



### Navigating to a given path

In an application we would like to navigate to a given path. We can do it in couple of ways.

1. **Wrapping with Link tag**

   ```react
   <Link to={"/"+post.id } key={post.id}> <!-- *** -->
       <Post
       title={post.title}
       author={post.author}
       clicked={() => {
       this.postSelectedHandler(post.id);
       }}
       />
    </Link>
   ```

   

2. **Programmatically**

   This is important if you want to navigate after processing something.

   ```react
   this.props.history.push({pathname:'/'+id});
   //or
   this.props.history.push('/'+ id);
   ```



### Nested Routes

We might have situation where we want to load a page inside another route. We can make use of route related props for that. We can make it absolute and dynamic by using `this.props.match.url`. We can load the routed component anywhere we want but it must be wrapped with <BrowserRouter>.

Make sure to use `componentDidUpdate` with nested routes because `componentDidMount` will be called only once.

```react
//Posts.js
return(
	<Post
        key={post.id}
        title={post.title}
        author={post.author}
        clicked={() => {
        this.postSelectedHandler(post.id);
        }}
    />
    <Route path={this.props.match.url+'/:id'} exact component={FullPost}}> <!-- // /posts/3 -- > <!--***-->
);
```



### Redirect

We can redirect to given url using react-router-dom. We need to used it in **Switch** tag.

```react
<Switch>
    <Route path="/new-post" exact component={NewPost} />
    <Route path="/posts" exact component={Posts} />
    <Route path="/" exact component={Posts} /> <!-- *** -->
</Switch>
```

OR

```react
<Switch>
    <Route path="/new-post" exact component={NewPost} />
    <Route path="/posts" exact component={Posts} />
    <Redirect from="/" to="/posts" /> <!-- *** -->
</Switch>
```

If **Redirect** is used outside **Switch**, we cannot provide `from` property. It will always redirect to the given path.

**Conditional redirects**

```react
render() {
    let redirect = null;
    if( this.state.submitted) {
        redirect = <Redirect to="/posts" />
    }
    return (
        <div>
        	{redirect}
            <h1>Add a Post</h1>
        </div>
    );
}
```

We can also use the **history** props to achieve the redirect capability programmatically.

```react
this.props.history.replace('/posts'); 
//Works similar to redirect, it replaces the current path

//or

this.props.history.push('/posts'); 
//It adds posts to top of the stack, so clicking back will navigate to the same place where it was called from
```

We don't have any specific **Navigation guards** provided in react-router-dom. We can simply check conditionally if we want to render the **Route** component.

```react
<Switch>
    {this.state.auth ? <Route path="/new-post" exact component={NewPost} /> : null} <!-- *** -->
    <Route path="/posts" exact component={Posts} />
    <Redirect from="/" to="/posts" /> <!-- If it doesn't find match route it redirects --> 
</Switch>
```

or We can simply check for authentication in `componentDidMount` and redirect..

```react
componentDidMount () {
    if(!this.state.auth) { //***
        this.props.history.replace('/posts');
    }
}
```

**Handling 404 cases**

```react
<Switch>
    <Route path="/new-post" exact component={NewPost} />
    <Route path="/posts" exact component={Posts} />
    <Redirect from="/" to="/posts" /> <!-- *** -->
</Switch>
```

or Don't provide the `path` property on **Route** component.

```react
<Switch>
    <Route path="/new-post" exact component={NewPost} />
    <Route path="/posts" exact component={Posts} />
    <Route component={Posts} /> <!-- *** -->
</Switch>

//(or)

<Switch>
    <Route path="/new-post" exact component={NewPost} />
    <Route path="/posts" exact component={Posts} />
    <Route render={() => <h1>Not found</h1>} /> <!-- *** -->
</Switch>
```



### Lazy loading/ Code splitting

In a large application, downloading all resources at once might increase the page load time. In order to overcome this **React** provides a capability to lazy load the required component when needed. This will make **Webpack** to render the required resource only at the time of that component rendering. 

**Lazy loading with hoc asyncComponent**

```react
// hoc/asyncComponent.js

import React, { Component } from "react";

const asyncComponent = (importedComponent) => {
    return class extends Component {
        state = {
            component: null
        }
        componentDidMount() {
            importedComponent()
            .then(cmp => {
                this.setState({component: cmp.default}); //***
            })
        }
        render() {
            const C = this.state.component;
            return C ? <C {...this.props} /> : null; //***
        }
    }
}

export default asyncComponent;


//Blog.js
import asyncComponent from "../../hoc/asyncComponent";
// import NewPost from "./NewPost/NewPost";
const AsyncNewPost = asyncComponent(() => { //*** Importing asynchronously when newpost gets rendered
  return import("./NewPost/NewPost"); //*** 
});


return (
    <Switch>
        <Route path="/" exact component={Posts} />
        <Route path="/new-post" exact component={AsyncNewPost} /> <!-- *** -->
        <Route path="/:id" exact component={FullPost} />
    </Switch>
)
```

After React 16.6, React provides the capability to lazy load in its library.

**Lazy loading with React.lazy**

It needs to be rendered inside <Suspense> tag. It has `fallback` property which will display untill React renders the component. It can be rendered anywhere we want not only in route.

```react
import React, { Component, Suspense } from "react"; //***
const NewPosts = React.lazy(() => import('./NewPost/NewPost')); //***
                            
return (
	<Route path="/posts" exact component={Posts} />
	<Route path="/new-post" exact render={() => (
        	<Suspense fallback={<div>Loading ...</div>}>
                <NewPosts />
            </Suspense>
        )} /> 
)

//One more usecase
import React, { Component, Suspense } from "react"; //***
import User from "./User/User";
const NewPosts = React.lazy(() => import('./NewPost/NewPost')); //***

class App extends Component {
    state ={showPosts: false};
	modeHandler = () => {
        this.setState(prevState => {
            return {showPosts: !prevState.showPosts}
        })
    }
    
    render() {
        return (
        	<button onClick={this.modeHandler}>Toggle Mode</button>
            {
                this.state.showPosts ? 
                (<Suspense fallback={<div>Loading ...</div>}>
                	<NewPosts />
            	</Suspense>) : <User />
            }
        )
    }
}
```



### withRouter

If at all we get into error with **redux** `connect` and **router**, wrap it hoc **withRouter**.

```react
import { Route, Switch, withRouter } from "react-router-dom";

export default withRouter(connect(null, mapDispatchToProps)(App)); //***
```



### Routing on Server

In production, or when the react application is deployed on any different server other than development server. Configure    the 404 requests to be forwarded to react application(index.html). The reason for this is, the routing is handled within the react application so the server might not understand the application routing. If the URL still cannot be mapped then react application will handle based on its app routing configuration.

This is not needed in dev setup because it is configured properly.

![](C:\ravi\Training\Notes\React\images\routing_server_deployment.PNG)

If you want load from a particular directory on the server, configure the `basename` with appropriate path.

```react
<BrowserRouter basename="/my-app"></BrowserRouter>//***
```





## Testing

![](C:\ravi\Training\Notes\React\images\testing_tools.PNG)

**Test Runner**: Jest. It is installed by default.

https://jestjs.io/docs/en/getting-started.html

**Testing Utilities** : React Test Utils/ Enzyme. React provides **React Test Utils**. **Enzyme** is developed by AirBnb.

```powershell
npm install --save enzyme react-test-renderer enzyme-adapter-react-16
```

https://enzymejs.github.io/enzyme/

Enzyme will help load components individually without rendering child components.

#### Configuring Enzyme

```react
//NavigationItems.test.js
import { configure, shallow } from "enzyme";
import Adapter from "enzyme-adapter-react-16";

configure({ adapter: new Adapter() });
```

You will need to configure Enzyme to support appropriate react version.

#### Unit testing

We should try to restrict the testing for a component. If it works as component it should also work when it is rendered as a child or through route.

1. Functional components

   ```react
   import React from "react";
   import { configure, shallow } from "enzyme";
   import Adapter from "enzyme-adapter-react-16";
   
   import NavigationItems from "./NavigationItems";
   
   configure({ adapter: new Adapter() });
   
   describe("<NavigationItems />", () => {
       let wrapper;
       beforeEach(() => {
           wrapper = shallow(<NavigationItems />); //***
       });
     it("should render all the links if authenticated", () => {
       //wrapper = shallow(<NavigationItems isAuthenticated />);
       wrapper.setProps({isAuthenticated: true}); //***
       expect(wrapper.find("li")).toHaveLength(3);
     });
   });
   ```

   `shallow` function from enzyme will help load the component without rending the children.

   

2. Containers

   Make sure you export class based component as named component just avoid the issue with **redux** `connect` before testing class based containers. Also it will help testing containers similar to functional components.

   Anyways you need ensure to `export default` to keep the app working.

   ```react
   export class BurgerBuilder extends Component { //***
   }
   export default connect(mapStateToProps,mapDispatchToProps)(BurgerBuilder);
   ```

   

   We can set the props and functions mapped by redux manually so it doesn't throw any error.

   ```react
   import React from "react";
   import { configure, shallow } from "enzyme";
   import Adapter from "enzyme-adapter-react-16";
   
   import { BurgerBuilder } from "./BurgerBuilder";
   import BuildControls from "../../components/Burger/BuildControls/BuildControls";
   
   configure({ adapter: new Adapter() });
   
   describe("<BurgerBuilder />", () => {
     let wrapper;
     beforeEach(() => {
       wrapper = shallow(<BurgerBuilder initIngredients={() => {}}/>); //***
     });
   
     it("should render <BuildControls /> when receiving ingrdients", () => {
       wrapper.setProps({ ingredients: { salad: 0 } }); //***
       expect(wrapper.find(BuildControls)).toHaveLength(1);
     });
   });
   ```

   

3. Redux (Reducers testing)

   Testing reducers is same as testing any normal function we don't even need to import React as we don't interact with JSX in reducers.

   ```react
   import reducer from "./auth";
   import * as actionTypes from "../actions/actionTypes";
   
   describe("auth reducer", () => {
     it("should return the initial state", () => {
       expect(reducer(undefined, {})).toEqual({
         token: null,
         userId: null,
         error: null,
         loading: false,
         authRedirectPath: "/",
       });
     });
   
     it("should store the token upon login", () => {
       expect(
         reducer(
           {
             token: null,
             userId: null,
             error: null,
             loading: false,
             authRedirectPath: "/",
           },
           {
             type: actionTypes.AUTH_SUCCESS,
             idToken: "some-token",
             userId: "some-userId",
           }
         )
       ).toEqual({
         token: "some-token",
         userId: "some-userId",
         error: null,
         loading: false,
         authRedirectPath: "/",
       });
     });
   });
   
   ```

   

## Deployment

![](C:\ravi\Training\Notes\React\images\deployment.PNG)

**Static servers**: AWS S3, Github pages and Firebase

#### Deploying on firebase

**On Firebase  ** *Go to project > Hosting* you will find the steps for deploying.

https://firebase.google.com/docs/cli?authuser=0#windows-npm

##### Step 1: Installing firebase-tools globally

```powershell
npm install -g firebase-tools
```

##### Step 2:  Firebase login

```
firebase login
```

##### Step 3: List the project to check if the login is right.

```
firebase projects:list
```

##### Step 4: 

```
firebase init
```

##### Step 5: You can select the options what you want by clicking `space` against the option. For hosting just choose the below option. and hit Enter.

```
Hosting: Configure and deploy Firebase Hosting sites
```

##### Step 6: Select the project where you want to host.

##### Step 7: You need to enter the folder which you to host. In our case it is `build`.

```powershell
? What do you want to use as your public directory? build
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
? Set up automatic builds and deploys with GitHub? No
? File build/index.html already exists. Overwrite? No
```

##### Step 8: 

```
firebase deploy
```

Project Console: https://console.firebase.google.com/project/react-burger-app-24f62/overview
Hosting URL: https://react-burger-app-24f62.web.app