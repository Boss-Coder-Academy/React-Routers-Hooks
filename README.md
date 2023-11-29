# React(Hooks, Routers)

## React routers
       
### 1. Introduction to React Router

React Router is a library that enables routing in a React application, allowing you to navigate between different components while maintaining a single-page experience. It provides a declarative way to define navigation and manage the application's URL.

### 2. Installation and Setup

To use React Router, you typically install it via npm or yarn:

``` bash
Copy code
npm install react-router-dom
# or
yarn add react-router-dom
```
Then, in your code, you'll import components like BrowserRouter, Route, Link, and others from 'react-router-dom'.

### 3. Basic Routing

+ **`<Route>`**:
The <Route> component is used to define a mapping between a URL path and a React component to render when the path matches.

+ **`<Link>`**:
The <Link> component creates a navigation link to different routes, preventing the browser from reloading the page.

**Example:** 

You can set up basic routing in your app using the BrowserRouter, <Route>, and <Link> components:

```javascript
// App.js
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';
import Home from './components/Home';
import About from './components/About';

const App = () => {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
          </ul>
        </nav>

        <Route path="/" exact component={Home} />
        <Route path="/about" component={About} />
      </div>
    </Router>
  );
};
```


**Explanation:**

+ `<Router>` wraps the entire app, enabling routing functionality in React.
+ `<Link>` creates clickable links to navigate between different routes.
+ `<Route>` components define what component should render for a specific URL path.
+ exact attribute ensures that only when the path exactly matches '/' it renders the Home component.





### 3. Dynamic Routing and Route Parameters

Route parameters allow you to extract values from the URL. For instance, defining a route like `/users/:id` would capture the id value from the URL.

**Example:**

```javascript
Copy code
// App.js
import { BrowserRouter as Router, Route } from 'react-router-dom';
import User from './components/User';

const App = () => {
  return (
    <Router>
      <div>
        <Route path="/users/:id" component={User} />
      </div>
    </Router>
  );
};
```

**Explanation:**

+ Route with `/:id` captures any value after `/users/ `as id.
+ The User component accesses this id parameter using the useParams hook.


### 4. Nested Routes and `<Switch>`

Nested routes involve having routes within other routes, enabling a hierarchy of components to be rendered based on the URL path. `<Switch>` allows rendering only the first matching `<Route>`.

**Example:**

```javascript
Copy code
// App.js
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Profile from './components/Profile';
import Settings from './components/Settings';

const App = () => {
  return (
    <Router>
      <div>
        <Switch>
          <Route path="/profile" component={Profile} />
          <Route path="/settings" component={Settings} />
        </Switch>
      </div>
    </Router>
  );
};
```

### 5. Programmatic Navigation

React Router provides the history object to enable programmatic navigation. You can push, replace, or go back in the history stack programmatically.

**Example:**


```javascript
Copy code
// SomeComponent.js
import { useHistory } from 'react-router-dom';

const SomeComponent = () => {
  const history = useHistory();

  const handleClick = () => {
    history.push('/new-route');
  };

  return (
    <div>
      <button onClick={handleClick}>Go to New Route</button>
    </div>
  );
};

```

### 6. Route Guards and `<Redirect>`

Route guards involve adding logic to protect certain routes, typically used for authentication purposes. `<Redirect>` is used to redirect users to specific routes programmatically.

**Example:**

Adding guards to routes to handle authentication:

```javascript
Copy code
// PrivateRoute.js
import React from 'react';
import { Route, Redirect } from 'react-router-dom';

const PrivateRoute = ({ component: Component, isAuthenticated, ...rest }) => (
  <Route
    {...rest}
    render={(props) =>
      isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />
    }
  />
);

export default PrivateRoute;
```

**Usage:**

```javascript
Copy code
// App.js
import { BrowserRouter as Router, Route } from 'react-router-dom';
import PrivateRoute from './PrivateRoute';
import Dashboard from './components/Dashboard';

const App = () => {
  const isAuthenticated = true; // Check user authentication status

  return (
    <Router>
      <div>
        <Route path="/login" component={Login} />
        <PrivateRoute
          path="/dashboard"
          component={Dashboard}
          isAuthenticated={isAuthenticated}
        />
      </div>
    </Router>
  );
};

```


### 7. Navigate

The `Navigate` component in React Router DOM (v6) is used for programmatic navigation, similar to how you might use `history.push` in earlier versions of React Router.

```javascript
import { Navigate } from 'react-router-dom';

<Navigate to="/dashboard" replace={false} state={null} />;
```

+ **Attributes**

- **`to` (string | Location):** Specifies the path to navigate to.
- **`replace` (boolean):** If `true`, it replaces the current entry in the history stack. Default is `false`.
- **`state` (object):** An optional state to be passed along with the navigation.

**Example Usage**

```javascript
import { Navigate } from 'react-router-dom';

const CustomComponent = () => {
  // Some condition that triggers navigation
  const condition = true;

  return condition ? <Navigate to="/dashboard" replace={false} /> : <OtherComponent />;
};
```

**Explanation**

+ The `<Navigate>` component allows for declarative, conditional navigation in React Router v6.
+ It can be used within components to conditionally trigger navigation based on certain conditions, such as successful login or other user actions.
+ It's useful for redirecting users to different routes or handling navigation based on specific logic within components.
+ The `to` attribute specifies the destination route, `replace` controls whether to replace the current history entry, and `state` allows passing additional data along with the navigation.

This component offers a clean and declarative way to handle programmatic navigation within React Router v6 applications.




## React Hooks

### 1. Introduction to React Hooks

React Hooks are functions that enable functional components to use state, lifecycle methods, and other React features. They were introduced in React 16.8 as a way to write more concise and reusable code in functional components


a) **useSate** 

The `useState` hook is a fundamental React hook used for managing state within functional components. It allows you to add stateful logic to functional components without using class-based components.


```javascript
import React, { useState } from 'react';

const Component = () => {
  const [state, setState] = useState(initialState);
  
  // Rest of the component logic
};
```

+ `useState` is a function that returns an array with two elements: the current state value (`state`) and a function (`setState`) to update that value.
+ `initialState` is the initial value of the state.

**Example**

Let's create a simple counter component to demonstrate `useState`:

```javascript
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>Counter</h2>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};

export default Counter;
```

+ `useState(0)` initializes the `count` state to `0`.
+ `setCount` is used to update the `count` state. It accepts a new value and triggers a re-render of the component with the updated state value.
+ `increment` and `decrement` are functions that call `setCount` to modify the `count` state.

**Explanation**

+ **Initializing State:** `useState` is used to initialize state within a functional component by providing an initial value. It returns an array with the current state value (`count`) and a function to update that state (`setCount`).
+ **Updating State:** `setCount(newValue)` updates the state value. It can take a new value or a function that returns a new value based on the previous state.
+ **Rendering:** When state changes, React re-renders the component, updating the UI to reflect the new state.
+ **Multiple State Variables:** You can use `useState` multiple times in a component to manage different state variables.

**Additional Notes**

+ The argument passed to `useState` is the initial state, which can be a primitive value (e.g., number, string, boolean) or a function that returns an initial value.
`useState` does not merge state like `this.setState` in class components. Instead, it replaces the entire state with the new value provided.

+ The `useState` hook is powerful for managing local component state in functional components, allowing for a simpler and more concise way to work with stateful logic. Adjust the example and explore further based on your application's state management needs.


b) **useEffect**


`useEffect` hook in React is used to perform side effects in functional components. It allows you to execute code in response to component lifecycle events, such as mounting, updating, and unmounting.

```javascript
import React, { useEffect } from 'react';

const Component = () => {
  useEffect(() => {
    // Effect code here
    return () => {
      // Cleanup code (optional) - runs on component unmount
    };
  }, [dependencies]);
  
  // Rest of the component logic
};
```

`useEffect` accepts two arguments: a function containing the effect and an optional array of dependencies.
The effect function can contain code that runs on component mount and whenever any of the dependencies change.
The optional cleanup function within the effect runs when the component unmounts or before the effect runs again.

**Example**

Let's create a component that fetches data when it mounts and performs cleanup when it unmounts:

```javascript
import React, { useState, useEffect } from 'react';

const DataFetcher = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data');
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    fetchData();

    return () => {
      // Cleanup function (optional) - runs on component unmount
      // Perform cleanup or unsubscribe here
    };
  }, []); // Empty dependency array ensures the effect runs only once on mount

  return (
    <div>
      {data ? (
        <div>Data: {JSON.stringify(data)}</div>
      ) : (
        <div>Loading...</div>
      )}
    </div>
  );
};

export default DataFetcher;
```

`useEffect` is used to fetch data when the component mounts using an asynchronous function.
The cleanup function, returned from the effect, would be used to clean up resources (e.g., subscriptions) or perform any necessary cleanup actions before the component unmounts.

**Explanation**

+ **Side Effects:** `useEffect` is suitable for performing side effects like data fetching, subscription handling, or DOM manipulation.
+ **Dependency Array:** The dependency array `[dependencies]` contains variables that, when changed, will trigger the effect to run again. If empty (`[]`), the effect runs only once (on mount) and never re-runs.
+ **Cleanup:** The optional cleanup function within the effect is useful for cleaning up resources or performing actions when the component unmounts.



**Additional Notes**

+ Effects without cleanup functions will run every time the component mounts or updates, potentially causing memory leaks or unnecessary re-renders. Cleanup functions are essential for cleanup and optimization.
Avoid excessive or unnecessary side effects within `useEffect` to ensure optimal performance.

+ `useEffect` is crucial for managing side effects in functional components and provides a way to handle lifecycle-related logic in React. Experiment with different dependencies and effect scenarios based on your application's needs.



c) **useLocation**
useLocation provides access to the current URL location.

d) **useRouteMatch**
useRouteMatch helps in accessing the match object for the closest `<Route>`.



## Advanced Concepts

### 1. Code Splitting

Code splitting involves breaking down your bundle into smaller chunks so that components or features are loaded only when needed, optimizing initial load times.

**Example:**

```javascript
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

const App = () => {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
};

export default App;
```

+ `React.lazy` allows for lazy loading a component. It dynamically imports the component when it's needed, reducing the initial bundle size.
+ `<Suspense>` is used to wrap the lazy-loaded component and provide a fallback UI (such as a loading indicator) while the component is being loaded.

### 2. Lazy Loading

Lazy loading refers to loading resources (such as components, images, or data) asynchronously, typically to improve performance by deferring the loading of non-essential content.

**Example:**

```javascript
import React, { useState, useEffect } from 'react';

const LazyLoadedComponent = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    import('./data.json').then((result) => {
      setData(result);
    });
  }, []);

  return (
    <div>
      {data ? (
        <div>Data: {JSON.stringify(data)}</div>
      ) : (
        <div>Loading...</div>
      )}
    </div>
  );
};

export default LazyLoadedComponent;
```

In this example, the `data.json` file is lazily loaded using dynamic `import`. This prevents the file from being loaded until the component requests it, improving initial load time.

### 3. Server-Side Rendering (SSR)

Server-side rendering involves rendering the initial HTML on the server before sending it to the client. It provides faster initial load times and better SEO.

**Example (with Next.js - a popular SSR framework for React):**

```javascript
// pages/index.js
import React from 'react';

const Home = () => {
  return (
    <div>
      <h1>Hello, SSR!</h1>
    </div>
  );
};

export default Home;
```

With Next.js, by default, pages are server-rendered. The HTML content is pre-rendered on the server and sent to the client, enhancing performance and SEO.


**Explanation:**

+ **Code Splitting and Lazy Loading:** These techniques help in optimizing load times by splitting your code into smaller chunks and loading them only when necessary, reducing the initial bundle size and improving user experience.

+ **Server-Side Rendering:** SSR generates HTML content on the server, sending it to the client, which results in faster initial render times and better search engine optimization as search engines can crawl the page content easily.

These advanced concepts in React help improve performance, user experience, and SEO by optimizing the loading of components, resources, and initial page content. Implementing them appropriately can significantly enhance the performance and scalability of React applications.



## Assignment:

**Task1**: Use react hooks in order to implement the same like/unlike functionality as discussed in previous assignments.

**Desired Output:**

![alt Image](https://i.postimg.cc/kGHzKB35/dasc.jpg)



**Code:** 

+ Make a ‘components’ folder inside the ‘src’ and create a file named header.js inside this components folder as shown:


![alt Image](https://i.postimg.cc/vB4ZsxzY/install.jpg)



+ Also, create a ‘utils’ folder inside ‘src’ and a subfolder called ‘images’ inside utils and add all the required images in this folder.

![alt Image](https://i.postimg.cc/3JnVgfhq/image.png)



+ Make sure that Tailwind is installed in this project.
 
+ Now write the following code in different files as follows

**Header.js**


```javascript
import React, { useState, useEffect } from 'react';
import cardImage from '../utils/images/card.jpeg';
import playButton from '../utils/images/playbutton.png';
import likeButton from '../utils/images/white-thumbs-up-icon-26.jpg';
import DislikeButton from '../utils/images/white-thumbs-down-icon-26.jpg';




const VideoCard = () => {
  const [likeCount, setLikeCount] = useState(0);


  // useEffect to monitor changes in likeCount
  useEffect(() => {
    // You can perform actions here when likeCount changes, if needed
    // console.log('Like count changed:', likeCount);
  }, [likeCount]);


  const handleLikeClick = () => {
    // Increment likeCount by 1 when the like button is clicked
    setLikeCount(likeCount + 1);
  };


  return (
    <div className="max-w-xs rounded overflow-hidden shadow-lg bg-gray-900 m-4">
      {/* Video Thumbnail */}
      <img
        className="w-full h-40 object-cover object-center"
        src={cardImage}
        alt="Video Thumbnail"
      />
     
      {/* Video Information */}
      <div className="px-4 py-2">
        <div className='flex '>
          <div className="font-bold text-xl mb-2 text-white">
            <button><img className='w-12' src={playButton} alt="Play Button" /></button>
          </div>
          <button onClick={handleLikeClick}>
            <img className='w-7 mr-2 mb-4' src={likeButton} alt="Like Button" />
          </button>
          <button><img className='w-7 mb-4' src={DislikeButton} alt="Dislike Button" /></button>
        </div>
        <p className="text-gray-300">{likeCount} likes</p>
        <div className='flex'>
          <p className=" mr-2 text-green-500">97% Match</p>
          <p className="text-gray-300 text-base">2h 35m</p>
        </div>
        <div>
          <ol className='flex'>
            <li className="text-gray-300 text-base mr-2">Horror</li>
            <li className="text-gray-300 text-base mr-2">Adventure</li>
            <li className="text-gray-300 text-base">Mystery</li>
          </ol>
        </div>
        {/* Display the like count */}
      </div>
    </div>
  );
};


export default VideoCard;

```


**App.js**

```javascript
import './App.css';
import Header from './components/header';


function App() {
  return (
    <div>
      <Header/>
      
    </div>
  );
}


export default App;
```


**Task2**: Create a modal as shown on clicking the more info button, which should pop up, on clicking more info, and close on clicking close button, as shown below:

**Desired Output:**

![AltImage](https://i.postimg.cc/pLMhJCBD/image.png)





