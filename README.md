## excess-router

Express-style routing for Polymer

## Example
```html
<my-app>

  <excess-route
    route="/:topmenu"
    topmenu="{{appMenu}}"
  ></excess-route>
  <excess-route route="/(.*)" redirect_to="/first"></excess-route>

  <iron-pages selected="{{appMenu}}" attr_for_selected="name">
    <first-page name="first"></first-page>
    <second-page name="second"></second-page>
  </iron-pages>

</my-app>
```

## Features

`excess-router` is a standard full-featured router, implementing
routing functionality also found in Angular, React, page.js.

- two Polymer elments: `<excess-route>` and `<excess-router-config>`

- Javascript library: `Excess.RouteManager`

- [express-style](https://github.com/component/path-to-regexp) path matching
```
  '/users/:id', '/:topmenu/:submenu?', '/(.*)'
```
- route aliases:
```javascript
  <excess-route route="/:topmenu" route-alias="main">
  Excess.RouteManager.navigateTo('@main', {topmenu: '0'});
```

- redirection
```html
  <excess-route route="/*" redirect_to="/
```

- route transition lifecycle callbacks: willDeactivate, willActivate

- anchor tag links are routed
```
  <a href="/#mainmenu"></>
```

- configurable
  - '#' hash, or '/' path style paths.
  - hashPrefix
  - manual start

- lazy display/loading when you use [lazy-pages](https://github.com/atotic/lazy-pages)

## How to use

`<excess-route>` binds window's URL to its attributes.
Route keys will be bound to `<excess-route>`'s properties. For example:
```html
<my-app>
  <excess-route route="/users/:userId" user-id="{{appUserId}}">
</my-app>
```
`:userId` key match will propagate to `excess-route.userId`, and `my-app.appUserId`.

When window.location is `#/users/1`
- `excess-route.userId` be 1
- `my-app.appUserId` will be 1
- setting `my-app.appUserId` to 999 will cause
  - `excess-route.userId` to become 999
  - which causes excess-route to call `Excess.RouteManager.navigateTo('/users/999')`
  - which changes window.location to `#/users/999`

Once route's keys have been reflected to your app, use Polymer's bindings to connect your route to `<iron-pages>`,
`<neon-animated-pages>`, `<lazy-pages>`, or whatever your route uses for navigation.

Not all routing use cases are covered just by using `<excess-route>`. If your problems become complex, you can drop down into Javascript and
use `Excess.RouteManager` library and use your JS superpowers.

## Example

How would you use this in Polymer Starter Kit?

```html
<my-app>
  <iron-route route="/:topmenu/:submenu"
    topmenu="{{appRoute}}" submenu="{{submenu}}"></iron-route>
  <iron-route route="/(.*)" redirect-to="/home"></iron-route>
  <paper-menu attr-for-selected="route" selected="{{appRoute}}">
    <a href="#/home" route="home">Home</a>
    <a href="#/users/all" route="users">Users</a>
    <a href="#/contact" route="contact">Contact</a>
  </paper-menu>
  <iron-pages attr-for-selected="route" selected="{{route}}">
    <section route="home">...</section>
    <section route="users">
      <iron-pages attr-for-selected="user" selected="{{submenu}}">
        <section user="all">
          <p>This is the users section</p>
            <a href="#/users/Rob">Rob</a>
        </section>
        <section user="Rob">
          User:<span>{{submenu}}</span>
        </section>
      </iron-pages>
    </section>
  </iron-pages>
</my-app>
```

## Notes

Why excess-router? Because it uses [pathToRegexp](https://github.com/component/path-to-regexp), same route parsing library as express, and
it has an excess of features.

My routing needs are simple. If you have some interesting ones, and
would like to figure out how to use this element for it, give me
a shout.

Intially, `<excess-route>`s could be nested. It more than doubled the code size, and I could not come up with a good use case. I've decided
that what I really want is nesting built into the core library.

