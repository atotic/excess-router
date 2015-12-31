0.9.12
==================
* bug: #10 parameter navigation sets optionals to undefined if not present

0.9.11
==================
* feature: RouteManager.getRouteUrl(path, params)

0.9.10
==================
* bugfix: #6: IE does not work when hash is changed by hand

0.9.9
==================
* bugfix: anchor click routing, no routing of right-clicks
* bugfix: replaceState was pushing, not replacing
* feature: add transitionStyle property to excess-route for cleaner history
Lets you specify history.push vs history.replace for attribute-triggered transitions.
* bugfix: uriDecode route matches
* bugfix: throw errors on illegal route transitions (during activate/deactivate callbacks)
* bugfix: redirect replaces the url, instead of doing a transition

0.9.8
==================
* bugfix: #2 initialize excess-route.active to false
* bugfix: setting excess-route.redirect-to after route has been activated triggers redirect

0.9.7
==================
* bugfix: activate multiple routes when transitioning via route
* bugfix: excess-route._setAndReflectRouteParams can be called recursively, guard against it

0.9.6
==================
* bugfix: changing routes throws null exception

0.9.5
==================
* bugfix: IE did not work: toString.call does not exist

0.9.4
==================
* bugfix: activate route on register if router started
* bugfix: routeparameters reflection (again, that /g flag)

0.9.3
==================
* bugfix: routeparameters reflection
* bugfix: navigation with basepaths
* bugfix: camelCase attributes were not firing correct change events
* fixed and updated the demo to Polymer Starter Kit

0.9.2
==================
* bugfix: _setRouteFromLocation did not update _destinationPath
* bugfix: excess-route: setting parameter to undefined should not cause navigation

0.9.1
==================
* bugfix: activationModifiers were swapped in excess-route

0.9.0
==================

* Add ability to activate multiple routes at once
  This allows for more 'natural' solutions to common routing problems:

    <excess-route route="/:topmenu">
    <excess-route route="/users">
    <excess-route route="/users/:userId">

* excess-route:
  - rip out old nested route code
  - add activationModifiers property
