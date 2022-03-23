# Quotes app

An application to write quotes and have a discussion around it.

We learned to use different routing features to give users an experience of navigation on different
pages while still leveraging all SPA conventions.

## migrating react router to v6

- replace `Switch` with `Routes`

- replace `Redirect` with `Navigate`
In this if you want to push the new page above current then do nothing, but if you actually want to
redirect from and replace the current page (default behaviour of `Redirect`) then add `replace` 
attribute on `Navigate` tag.

- convention to write `Route` is also changed instead of -
    ```
    <Route path={...path...}>
        <...element to be rendered...>
    </Route>
    ```
we have,
    ```
    <Route path={...path...} element={...element to be rendered...}>
    ```

- working in v5 => Route module used to look for matching route prefix to be able to decide
which route to load, hence we needed `exact` to compel Route module to look for exact route match.
but now,
working in v6 => Route module by default look for exact route match hence need for `exact` is gone.
hence, it has been removed.

and if you want to match the behavior of matching prefix, then in `path` while defining the path,
add `/*` after that path, e.g. - `path={/products/*}` instead of `path={/products}`

- In `NavLink` the attribute `activeClassName` has been removed instead, you will have to use commonly
known `className` attribute. This attribute when used on NavLink has been enhanced with a little 
more capabilities.

Now, you will have to write a function in it, which have a variable `isActive` that you can use to
apply active class, like this -
` className={(navData) => navData.isActive ? classes.active : ''} `

- way of doing nested routing has also changed, wherever you are defining a `Route`, encapsulate
it in `Routes`.

And, the route for which you want to do nesting, add `/*` (compulsorily) after declaring it, in path.
And furthermore, while nesting, routes become relative, so instead of ` paren/child/:id ` just write
` child/:id ` as for parent you alread did ` paren/* `, while defining the parent path.

Note: in nested routing the `Link` tag routes also become relative to parent component route.

Note: the route nesting can also be done as -
```
<Routes>
    <Route path={route1/*} element={<comp1 />}>
        <Route path={route2} element={<comp2 />} />
    </Route>
</Routes>
```
and then use `<Outlet/>` component to know the exact location of rendering of child route component in
parent route component.

- replace `useHistory` with `useNavigate`.
to navigate do `navigate('/route');` but if you want to redirect with replacing the current route in
history, do `navigate('/route', {replace: true})`.

also, you can use negative numbers to go back in history, e.g. - `navigate(-1)` to go back one page.

- `<Prompt>`, `usePrompt()`, `useBlock()` are removed, you need to write a work around for them by
yourself.

## Lazy loading

we load certain part of our code only when it is needed. We split our code into parts and bring a part to
the browser and load it only when it is needed. 

## Deployment steps -
- Test code
- Optimizations that can be done (lazy loading)
- Build app for production (to create a minified production ready bundle of our code)
    - run `npm run build`
    - you will get a minified bundle of chunks which you can now deploy
- Uploading production code to server
    - Everything which you build, after bundling, its a static website. Hence we need a `static site host`
      i.e., a hosting partner, which can serve static websites.
- Server configurations
    - configure servers properly to work with SPA static websites (look below for details).

```
Note: static website mean, that it is made only of HTML, CSS and JS. No server side code
is involved.
```

## difference between server-side routing and client-side routing

### client-side routing -
Once the React app is loaded the react-router code will analyze URL entered, and bring up corresponding
page/component to get rendered.

### server-side routing -
Whatever URL is requested by user (if it has some query params) the Server will look for page corresponding
to that URL only. Which is not the behaviour we want in SPA. In SPA, we want that whatever URL is entered,
server returns the index.html page (irrespective of what query-params and sub-routes are present), then,
once index.html page is loaded and application is in action then react-router code will judge the URL
automatically and will load the corresponding page/component.

So, we want if request is made for the first time for our website, then only domain name is taken into
account and index.html is returned by the server.

So all requesting URLs should be re-written to return index.html in case of an SPA.
