---
theme: style.css
verticalSeparator: -v-
highlightTheme: github
---

# React Router
## Sia Karamalegos

-v-

# React Router

Declarative routing for React

<img src="https://s3.amazonaws.com/viking_education/web_development/web_app_eng/US_route.gif" alt="US map with animated route">

-v-

## React Router TL;DR

With React Router, "routes" aren't really routes, they are just components.

It conditionally shows a component based on whether the current URL matches a given URL.

-v-

## What are we learning about?

### React Router version 4

- Installation
- Router, Routes, and Rendering
- Paths and Links
- Params and Queries
- Multiple Matching Routes
- Redirects

---

# Installation

<img style="border:0;" src="./images/heavy_node_modules.png" alt="Heaviest objects in the universe">

-v-

## Installation

To use React Router in a web application, first we need to install the **dom** version:

```bash
$ npm install react-router-dom --save
```

Different versions exist for different environments. For example, if you are building mobile apps in React Native, you would install `react-router-native`.

---

# Router, Routes, and Rendering

<img style="border:0;" src="./images/dog_riding_turtle.gif" alt="Dog riding a turtle">

-v-

## BrowserRouter

To build components that conditionally show depending on the current URL, we need to tap into the browser's native HTML5 [browser history API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) using `<BrowserRouter>`.

```javascript
// Import BrowserRouter as a named import
import {
  BrowserRouter as Router,
  Route,
} from 'react-router-dom'

const App = () => (
  <Router>
    <Route exact path="/" component={Home} />
  </Router>
)
```

-v-

## BrowserRouter

Child `<Route>` components receive 3 props:

- `match` - shows how the URL matched the given URL like the path, any params, and other info like whether the match was exact
- `location` - immutable representation of the location, or URL, containing attributes like pathname and search string.
- `history` - an object containing many properties and methods based on the [history package](https://github.com/ReactTraining/history). Often used for `history.push()` to programmatically "navigate" by adding a new URL to the history stack.

Note: `location` can also be accessed from `history` with `history.location`. You should not use this version of the location though as it is mutable. `BrowserRouter` is really just the name for the router implementation for HTML5 browsers. When we import it, we usually rename it to just `Router` for simplicity:

-v-

## BrowserRouter

Routers may only have one child element, so if you have other children, you can wrap them in a div or other element.

```javascript
// Import BrowserRouter as a named import
import {
  BrowserRouter as Router,
  Route,
} from 'react-router-dom'

const App = () => (
  <Router>
    <div>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
    </div>
  </Router>
)
```

-v-

## Route

<div style="text-align:left;">
  Each `<Route>` defines:
</div>
- the path to match against
- which component to render when the current URL, or location, matches the given path

-v-

## Rendering

We have 3 options for `<Route>` rendering props:

- `component` indicates which component to render for that route
- `render` must be a function that returns a component or element. Most convenient for short inline snippets.
- `children` (tricky) is a function like `render` but ALWAYS renders regardless of match. Convenient for animation and cases like modifying a className based on the route.

-v-

## Rendering Examples

```javascript
const Home = () => <h1>Home</h1>
const About = () => <h1>About</h1>

const App = () => (
  <Router>
    <div>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route
        path="/contact"
        render={() => <h1>Contact Us</h1>} />
      <Route path="/blog" children={({match}) => (
        <li className={match ? 'active' : ''}>
          <Link to="/blog">Blog</Link>
        </li>)} />
    </div>
  </Router>
)
```

---

# Paths and Links

<img style="border:0;" src="./images/super_pup.gif" alt="Dog in superman costume walking on treadmill">

-v-

## Path

The `path` prop is a string representing the URL to match against.

- Can be any string path that the [path-to-regexp](https://github.com/pillarjs/path-to-regexp) package understands.
- Is optional. If left out, that Route will always match.

-v-

## Tricksy Routes

- Remember, a `Route` is a component that renders when the current URL matches the given path.
- If more than one path matches, you get multiple rendered routes.
- Here both Home and About are rendered when at `/about`:

```javascript
const Home = () => <h1>Home</h1>
const About = () => <h1>About</h1>

const App = () => (
  <Router>
    <div>
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
    </div>
  </Router>
)
```

Note: This is because "/" is still a match for "/about", just not exact.


-v-

## Exact Matches

If we only want an exact match for "/" to render the Home component, so we simply add `exact` as a qualifier for the Route:

```javascript
const Home = () => <h1>Home</h1>
const About = () => <h1>About</h1>

const App = () => (
  <Router>
    <div>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
    </div>
  </Router>
)
```

Note: Another solution to this problem is to only render the first match for a route and order the routes accordingly...

-v-

## Switch

To only render the first matching route, use `<Switch>`.

- Handy for dealing with catch-all routes and tricky ones that might inadvertently match routes with params.
- Generally good to have a catch-all route like a 404 page:

```javascript
const Home = () => <h1>Home</h1>
const About = () => <h1>About</h1>

const App = () => (
  <Router>
    <div>
      <Link to="/">Home</Link>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route render={() => <h1>Page not found</h1>} />
      </Switch>
    </div>
  </Router>
)
```

Note: If we did not wrap our Routes in the Switch component, they would always render the last Route, even for the matching Home and About paths.

-v-

## Link

For internal links, use `<Link>` instead of  `<a>`.
- Instead of `href`, use the `to` prop.
- `to` accepts a string or an object with a property `pathname`.

```javascript
const Home = () => <h1>Home</h1>
const About = () => <h1>About</h1>

const App = () => (
  <Router>
    <div>
      <Link to="/">Home</Link>{' '}
      <Link to={{pathname: '/about'}}>About</Link>

      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route render={() => <h1>Page not found</h1>} />
      </Switch>
    </div>
  </Router>
)
```

-v-

## NavLink

To update styling for a link based on path match, use the `NavLink`. It accepts these extra props:

- `activeClassName` - a class name to add to the component if matched
- `activeStyle` - inline styles to apply if matched
- `isActive` - sometimes you may want to consider more information before categorizing a link as active. This is where you would customize the boolean condition.
- `exact` - to only do exact matches.

-v-

## NavLink examples

```javascript
const Home = () => <h1>Home</h1>
const About = () => <h1>About</h1>

const App = () => (
  <Router>
    <div>
      <NavLink exact to="/" activeClassName="active">
        Home
      </NavLink>{' '}
      <NavLink to="/about" activeStyle={{color: 'red'}}>
        About
      </NavLink>

      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route render={() => <h1>Page not found</h1>} />
      </Switch>
    </div>
  </Router>
)
```

---

# Params and Queries

<img style="border:0;" src="./images/confused_pugs.gif" alt="Pugs cocking their heads">

-v-

## URL Parameters

Add params using a colon then the name of the param.
- Define like `/resource/:id`
- Use like `/resource/42` which accesses the page for the resource with an id of 42.

```javascript
// Render links and declare the pages route
const App = () => (
  <Router>
    <div>
      <Link to="/page/1">Page 1</Link>{' '}
      <Link to="/page/2">Page 2</Link>{' '}
      <Link to="/page/3">Page 3</Link>

      <Route path="/page/:page_number" component={Page} />
    </div>
  </Router>
)
```

-v-

## URL Parameters

To access the params inside the rendered component, use `match` (provided by `BrowserRouter`) which has a `params` property.

```javascript
// Destructure match from props and access page_number
// param from match inside the component
const Page = ({match}) => (
  <h1>Page: {match.params.page_number}</h1>
)
```

For an param optional, add a question mark at the end:

```javascript
<Route path="/page/:page_number?" component={Page} />
```

Note: You can use regular expressions for more precision in matching, like validating that a param is a number.

-v-

## Query Links

- Queries use the URI standard of a question mark then the key-value pairs.
- Or, use the object option for the `to` and provide the `search` property.

```javascript
// Option 1: URI standard string queries
<Link to="/?name=olga">
  Search for Olgas
</Link>

// Option 2: `to` Object with `search` property
<Link to={{pathname: "/", search: "name=olga"}}>
  Search for Olgas
</Link>
```

-v-

## Accessing Query Params

Access the search query from the `search` property on the `location` object.

```javascript
// Query component stringifying the search object
const Query = ({location}) => (
  <h1>Query: {JSON.stringify(getParams(location.search))}</h1>
)

// Adding links for our 2 queries and the route for them
const App = () => (
  <Router>
    <div>
      <Link to="/?name=olga">Search for Olga in names</Link>{' '}
      <Link to={{pathname: "/", search: "country=sweden"}}>
        Search for Sweden in countries
      </Link>{' '}

      <Route path="/" component={Query} />
    </div>
  </Router>
)
```

Note: A number of packages exist for parsing URL strings. If you need something heftier than a helper method, check them out or try out the new [URLSearchParams API](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) available on Chrome.

---

# Multiple Matching Routes

<img style="border:0;" src="./images/synchro_tails.gif" alt="Synchronized tail wagging">

-v-

## One URL, Multiple Routes

You can render multiple components in different parts of your application based on the URL.

```javascript
const App = () => (
  <Router>
    <div>
      <SideBar />
      <Main />
    </div>
  </Router>
)
```

Note: For example, an item in the sidebar should change as well as the core content in the main section. Remember Routes are just components - they are checking for a match condition and then rendering whatever is in their component/render prop. It does not matter if we check for the matching condition in multiple places - each Route is independent of each other Route and will follow its own rules for rendering.

-v-

## One URL, Multiple Routes

```javascript
const Home = () => <h1>Home</h1>
const About = () => <h1>About</h1>

const Main = () => (
  <div>
    <Switch>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route render={() => <h1>Page not found</h1>} />
    </Switch>
  </div>
)

const SideBar = () => (
  <div>
    <Route exact path="/" render={() => <p>Welcome!</p>} />
    <Route path="/about" render={() => <p>About us...</p>} />
  </div>
)
```

---

# Redirects

<img style="border:0;" src="./images/cage_runner.gif" alt="Dog running in cage">

-v-

## Redirect

Use `<Redirect>` to replace the history with the new location (thus back button does not re-attempt the location triggering redirect). Props:

- `to` - the string or object (similar to Link) which indicates the new URL to redirect to.
- `from` - optional prop which is a pathname to redirect from, and can only be used if the Redirect is inside of a Switch.
- `push` - a boolean attribute (like `exact`) that when present changes the default behavior of the redirect to add the new URL to the history rather than replacing the from entry.

-v-

## Redirect Patterns

Developers use two main patterns for Redirect:

1. Defined in a `<Switch>` with both `from` and `to` props.
2. As a child of a `<Route>` which generally checks a logical condition to redirect with only a `to` prop.

```javascript
// Defined in a `<Switch>` with both `from` and `to` props
<Switch>
  <Redirect from='/old-path' to='/new-path'/>
  <Route path='/new-path' component={NewPathComponent}/>
</Switch>

// As a child of a `<Route>` which generally checks a
// logical condition to redirect with only a `to` prop
<Route path="/protected-page" render={() => (
  loggedIn ?
    <h1>Welcome logged in user!</h1> :
    <Redirect to="/login" />
)} />
```

-v-

## Params and Redirect

If you need to pass params from the old path to the new path, you can use the second pattern to grab them from the `match` data of the old route:

```javascript
// Preserving params in a redirect
<Route path="/puppy/:id" render={({match}) => (
  <Redirect to={`/dog/${match.params.id}`} />
)} />
```
