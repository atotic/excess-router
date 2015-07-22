Another attempt at creating a useful Polymer router.


#iron-route

`iron-route` ties window URL to your application's state. Example usage:
- select which `iron-page` to display
- store form data in the URL for sharing, bookmarks

`iron-route` supports modern router features:
- Express-style path matching
- aliases
- nested routes
- transition callbacks with abort: willDeactivate, willActivate
- navigation API: transitionTo, replaceWith

It is implemented on top of `Polymer.IronRouterManager` routing library.
The library can be configured with `iron-router-config` element.

### Simple example
```
  <template is="dom-bind">
    <iron-route route="/:topmenu" topmenu="{{selected}}"></iron-route>
    <iron-pages selected="{{selected}}">
      <div>Page 1</div>
      <div>Page 2</div>
      <div>Page 3</div>
    </iron-pages>
  </template>
```
This example will bind currently selected page to the URL. Changing the URL
will change the page, changing the page will change the URL.

### Defining the route

#### Express-style routes
Route is defined with `route` attribute. It is usually specified as an express-style string. Examples:

`/main/about` -- plain route

`/users/:id` -- route with routeParameters

`/users/(.*)` -- wildcard route

Route paramaters are reflected to element properties. Use Polymer bindings
to work with them. Example:

```
  <iron-route route="/topmenu/:selected" selected="{{mySelection}}"></iron-route>
```
This route would match `/topmenu/about`, `/topmenu/main`, etc. When route is active,
`selected` property will reflect the `:selected` match. You can bind to `selected` to change your
ui state depending on its value.

#### Aliases
Route can also be specifed as an alias, refering to previously defined route. Alias
routes are prefixed by '@' (configurable with iron-router-config)
Example:
```
  <iron-route route="/topmenu/about" alias="about"></iron-route>
  <iron-route route="@alias"></iron-route>
```
Second route is an alias for the first. Alias routes activate whenever original route is active, and can have their own transition callbacks.

### Navigation

Navigation is triggered by:
- window's URL change
- calling navigation routines: `transitionTo`, `replaceWith`
- changing active route's properties

When route changes, `IronRouteManager` searches registered routes for a new match. First matching route becomes new destination. Transition to new destination is initiated.

If route, or its child route, is active, changing it's route properties will trigger a
transition. In the first simple example above, changing `{{selected}}` triggers `iron-route`
to start transition to new destination.

### Transitions

Navigation initiates the transition from source to destination. Transition has 4 phases:

- source.willDeactivate
- destination.willActivate
- source.deactivate
- destination.activate

`iron-route` fires `iron-route-will-activate` and `iron-route-will-deactivate` events during transition. Transition can be cancelled by calling `ev.preventDefault()`.

Cancelling transitions is mostly used for forms with unsaved changes.

`IronRouteManager` offers asynchronous route transitions, but this functionality is not
exposed in `iron-route`

### Nested routes

Routes can be nested. Nested routes's url is a concatenation of its parents route, and given route. Nesting is useful when composing components.

Nested routes attribute reflection, and activation are slightly more complex than ordinary routes.

The main principles are:
- routes reflect only attributes directly defined, and not the parent's attributes.
- attribute navigation is enabled in parents of active routes. Changing parent's attribute
will navigate to route defined by parent, and not one defined by child.

Lets look at an example:

```
  <iron-route id="top" route="/users/:userId" user-id="id">
    <iron-route id="account" route="/account"></iron-route>
    <iron-route id="review" route="/review/:reviewId"></iron-route>
  </iron-route>
```

In the HTML below, normally invisible attributes will be shown for illustration purposes.

----
Current URL `/users/1`:
- "#top" is active.
- `#top.userId` is '1'
- changing `#top.userId` will trigger navigation because it is active

```
  <iron-route id="top" route="/users/:userId" user-id="1" active>
```
----
Current URL `/users/1/account`
- "#account" is active, because it is a nested route, and its full url is `/users/:userId/account`
- "#account.userId" is undefined. Nested routes only reflect properties defined by given route, not its parents.
- `#top.userId` is '1'. When child route is activated, the properties inherited from parent will be reflected in the parent.
- "#top" has an active child
- changing `#top.userId` will trigger navigation because it has an active child

```
  <iron-route id="top" route="/users/:userId" user-id="1" child-active>
    <iron-route id="account" route="/account" active>
```
----
Current URL `/users/1/review/5`
- "#review" is active, because nested, with full URL: `/users/:userId/review/:reviewId`
- "#review.reviewId" is '5'
- "#review.userId" is undefined
- "#top.userId" is '1'
- "#top.reviewId" is undefined
- "#top" has an active child
- changing '#review.reviewID" will trigger navigation
- changing '#top.userId' will trigger navigation
- changing "#top.reviewId" or "#review.userId" will not trigger navigation.
- if "#review.reviewId" is set to "99", new URL will be `/users/1/review99`
- if "#top.userId" is set to "2", new URL will be `/users/2` (child route is not used).

```
  <iron-route id="top" route="/users/:userId" user-id="1" child-active>
    <iron-route id="review" route="/review/reviewId" review-id="5" active>
```


