TODO:
- allow multiple routes to be active at once

- routeActivation:
  single (default for
  multiple (default)

ROADMAP:

PROBLEMS:
- named routes and nesting. Parent can't be a named route
- you can't have a wildcard matching route, and a named route that points to a particular match
  ex: wildcard /mainmenu/:selected
  named route /mainmenu/account
  fix: registering named route with a filter!
