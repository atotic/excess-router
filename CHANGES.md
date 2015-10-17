0.9.5
==================

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
