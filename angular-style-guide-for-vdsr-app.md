
#VDSR Client App Style Guide

###General

General guidelines that we will use for the angular app are subset of
the [style guide of John Papa](https://github.com/johnpapa/angularjs-styleguide).
I am linking to the parts that we will be using.

- [One component per file (single responsibility)](https://github.com/nozer/angularjs-styleguide#single-responsibility)
- [Wrap components in IIFE](https://github.com/nozer/angularjs-styleguide#iife)
- [Modules: naming, setting, getting, named anonymous functions](https://github.com/nozer/angularjs-styleguide#modules)
- Controllers: controller as syntax - we don't use them but study them to understand the examples used in this guide. [Here](https://github.com/nozer/angularjs-styleguide#style-y030), [here](https://github.com/nozer/angularjs-styleguide#style-y031), [and here](https://github.com/nozer/angularjs-styleguide#style-y032)
- [Controllers: bindable members to top](https://github.com/nozer/angularjs-styleguide#style-y033)
- [Controllers: function declarations to hide implementation details](https://github.com/nozer/angularjs-styleguide#style-y034)
- [Controllers: defer/delegate logic to services](https://github.com/nozer/angularjs-styleguide#style-y034)
- [Controllers: keep controllers focused](https://github.com/nozer/angularjs-styleguide#style-y037)
- [Services: know the difference between .service and .factory and use .factory to create service - unless you know what you are doing](https://github.com/nozer/angularjs-styleguide#style-y040)
- [Factories: single responsibility](https://github.com/nozer/angularjs-styleguide#style-y050)
- [Factories: accessible members to the top](https://github.com/nozer/angularjs-styleguide#style-y052)
- [Factories: function declarations to hide implementation details](https://github.com/nozer/angularjs-styleguide#style-y053)
- [Data Services: separate data calls to a factory](https://github.com/nozer/angularjs-styleguide#style-y060)
- [Data Services: return promise from data calls](https://github.com/nozer/angularjs-styleguide#style-y061)
- [Directives: one directive per file](https://github.com/nozer/angularjs-styleguide#style-y070)
- [Directives: manipulate dom in directive](https://github.com/nozer/angularjs-styleguide#style-y072)
- [Directives: provide a unique directive prefix](https://github.com/nozer/angularjs-styleguide#style-y073)
- [Directives: restrict to Element and Attributes](https://github.com/nozer/angularjs-styleguide#style-y074)
- [Controller activation promises & start-up logic](https://github.com/nozer/angularjs-styleguide#style-y080)
- [Route resolve promises](https://github.com/nozer/angularjs-styleguide#style-y081) (please note that we use ui-router but same logic applies)
- [Manually inject dependencies](https://github.com/nozer/angularjs-styleguide#style-y091)


###VDSR Specific

We will use the `clientapp/` directory for the javascript
and html files and the `assets/` directory for the
style files.

```
sails/
  ├── ...
  ├── clientapp/
  │   ├── account/
  │   │   ├── admin/
  │   │   │   ├── groups/
  │   │   │   │   ├── groups.ctrl.js
  │   │   │   │   ├── groups.partial.html
  │   │   │   │   ├── active-groups.ctrl.js
  │   │   │   │   ├── active-groups.partial.html
  │   │   │   ├── admin.ctrl.js
  │   │   │   ├── admin.partial.html
  │   │   ├── dashboard/
  │   │   ├── account.ctrl.js
  │   │   ├── account.partial.html
  │   │   ├── vdsr.config.js
  │   │   ├── vdsr.module.js
  │   │   ├── vdsr.run.js
  │   ├── account-common/
  │   │   ├── actions/
  │   │   ├── directives/
  │   │   │   ├── stw-user-list.js
  │   │   │   ├── stw-user-list.html
  │   │   │   ├── ...
  │   │   ├── filters/
  │   │   │   ├── repeat.js
  │   │   │   ├── ...
  │   │   ├── services/
  │   │   │   ├── ...
  │   │   ├── stores/
  │   ├── visitor/
  ├── ...
  ├── assets/
  ├── ...
```

`account` and `visitor` are two different ng applications.

`account` app runs after a user is logged in (member pages)
and the `visitor` app runs before a user logs in (guest pages).

`account-common` has the shared components for the `account` app and
chosen to be a sibling folder to the account folder
(as opposed to having a common folder under account)
to avoid having too much nesting (for ease of navigation).

####Directory Structure for `account`
- Controllers will have `.ctrl.js` extension and the partials
will have `.partial.html` extension.
- Name for the partial and controller will be the same (excluding extension).
For example: `groups.ctrl.js`, `groups.partial.html`.
- All the directories and sub directories under `account`
must reflect the name of the state.
- For example, given a state name like ``'account.admin.groups'``,
controller name for this state will be `groups.ctrl.js`
and the template for the controller will be named
`groups.partial.html` in a directory `account/admin/groups`
as shown in the directory structure above. Logic is
`<last word in state>.ctrl.js`,
`<last word in state>.partial.html`, and
``'<word1 is dir1>/<word2 is dir2>/<word3 is dir3>'`` for
controller, template, and directory, respectively.
- If a word in the state is hyphenated, then directory
for the related controller/template should be same as the word that precedes the first hypen.
- For example, given a state name like
`'account.admin.groups-active'`, controller, template, and directory
will be `groups-active.ctrl.js` (or `active-groups.ctrl.js`, whatever makes the most sense),
`groups-active.partial.html` (or `groups-active.partial.html`),
`account/admin/groups`, respectively.
- `vdsr` is the name of the module for the `account` ng app.
- `vdsr.config.js` includes the `state` info for `ui-router`.
- `vdsr.run.js` includes code to run when angular starts up such as definining the `state` redirects.
- `vdsr.module.js` defines the module for the app and its
dependencies.
- `account.ctrl.js` is the root controller for our app. You can place
common functions that are available to all controller scopes in here (Please note that
you must use this place very sparingly and in situations
where you cannot put those functions somewhere
in `account-common` directory)

####Info on `account/vdsr.config.js`
- If some data from a remote server must be preloaded to use in the controller /
template of the related `state`, then the data must be preloaded
via `resolve` config property of that `state`.

- All paths for the template partials must be prefixed
with `/partials` followed by directory structure starting
with `/account/...`. Because, `grunt` task runner will place all the template
html files in a public directory named `/partials` while also copying the directory structure.

- For example:

```
.state('account',{
  url:'/account',
  controller: 'AccountCtrl',
  templateUrl: '/partials/account/account.partial.html',
  resolve: {
    accountPreload: ['$q', 'groupActions','deviceActions',
          'roleActions', 'permActions', 'userActions',
       function($q, groupActions, deviceActions,
          roleActions, permActions, userActions){

       return $q.all([
          roleActions.loadRoles(),
          permActions.loadPermissions(),
          groupActions.loadGroups(),
          deviceActions.loadCompanyDevices(),
          userActions.loadCurrentUser()
       ]);
    }]
  }
})
```




####Controllers
- Put all the startup logic inside an `activate` function. This way, when we are
examining the code, we know where to look to see
what is being set up when this controller is instantiated.
- Keep it small and simple. Delegate work to directives to
reduce cognitive overload and to keep things manageable.

####Directory structure for `account-common`
- This directory contains shared components for the
`account` app defined in
`actions`, `directives`, `filters`, `services`, and
  `stores` directories.
- Each component directory will define its own module and
all the components in that directory will register on that module.
For example, all components in `services` directory will be registered on
module defined in `vdsr.services.module.js` in same directory.
Then, `vdsr` angular module
defined in `account/vdsr.module.js` will require these as dependencies.



####Directives
- Directives should be prefixed with `stw-`.
- Directive name should be camelCased and the relevant file should be hyphenated (i.e. `stwUserList` vs `stw-user-list.js`)
- There must be one javascript file and zero or more
template files per directive.
  If template file exists, it should have the same name as the
  js definition file with .html extension
  (`stw-user-list.js` vs `stw-user-list.html`)
- If more than one template files are needed for the same directive, one of the following conventions should be followed:
    - Put it in a separate file named by adding suffix to the base
    template file name (`stw-user-list-helper.html`)
    - Put it as an inline template inside the main template file
    as `script` tag with an id having the same name as the base template but with suffix added: i.e

    ```
    <script type="text/ng-template" id="stw-user-list-helper">...</script>
    ```

- Use `controller` instead of `link` in directive definition - unless link usage is a must
- Put all of the directives inside the `directives/` folder shown above.
- Main template file content for the directive must be wrapped
inside a div that has a css class with the same name as
the directive file name:
i.e. ```<div class="stw-user-list">...</div>```.
Point of it is to use that class as a namespace to style
this directive without interfering with others.

####Filters
- File name for the custom angular filters must be the same as the filter name.
- Place all filters in the `filters/` directory under
`account-common`

####Stores
- [flux-angular](https://github.com/christianalfoni/flux-angular) stores must be
placed under `stores/` directory under `account-common`.
- A `store` is a special type of service to hold data that are used
in different places throughout the application. If your
data is used only in one place (or used in different places
but you don't mind fetching it from the server in each of these places), just get that data
directly from that controller or directive (by using a service).
- `Stores` must modify its data (initialize, add, update, remove) by listening to
the relevant events from `actions`. It must export methods that only return data.
Do not try to directly manipulate the data of the store.
- Store file name should end with `-store.js` suffix (i.e. `user-store.js`)

####Actions
- If some user action, such as initial load of the page or
some interactions
on the user interface, will cause changes in a store,
then you must
create a factory service in the `actions/` directory and
trigger the relevant event from a method
(henceforth `action method`) in that
factory service so that the stores listening
to this event can update their data.
- Action file name should end with `-actions.js` suffix
(i.e. `user-actions.js`) and service name must be camelCased (i.e. `userActions`).
- The `action method` must return a promise (if doing async work).
- It should fire the relevant event after async operation is completed successfully.
- Directive or controller that called this `action method` should use the
promise returned to show progress indicator and errors (if any) after completion.
- For example: `actions/user-actions.js`

```
...
return {
   loadUsers: loadUsers,
   addUser: addUser,
   deleteUser: deleteUser,
   ...
};

// this is an action method
function loadUsers(){
   // returning a promise
   return api.getUsers().then(function(users){
      // triggering the relevant event after successful completion
      flux.dispatch(constants.actions.USER_POPULATE_SUCCESS, users);
      return users;
   });
}
...
```
- and `stores\user-store.js`

```
...
function storeDef($filter, constants, utils){

   var handlers = {};
   // listening to the relevant event and calling the indicated function
   // to update its data
   handlers[constants.actions.USER_POPULATE_SUCCESS] = 'populateSuccess';
   ...

   return {
      users: [],

      exports: {
         getAll: function(){
            return this.users;
         }
         ...
      },

      handlers: handlers,

      // here the function updates the data
      populateSuccess: function(data){
         this.users = angular.copy(data);
         this.emitChange();
      },

```

####Services
- Any service that is not `action` or `store` goes in `services/`.
- File name should reflect the name of the service. Service names,
if contains more than 1 word,
must be camelCased while the file name must be hyphenated
  (i.e. `self-ref.js` vs `selfRef`).
- `api.js` will contain all our server calls
- `constants.js` will contain `constants` such as event names or permission names.
- `globals.js` will hold key value pairs that are
set on server (by passing data to `views/layoutAdmin.js`) such as siteUrl.
- `ui.js` contains code regarding showing user interfaces elements such as dialog.
- `utils.js` contains generic useful functions.


####Directory structure for `assets/styles`
- Use `account.less` to style member pages (`account` app).
- Use `visitor.less` to style guest pages (`visitor` app).
- Use `global.less` to style both guest and visitor pages.
- If you must create a new style file, make sure it has `.less`
extension and do not forget to `@import` it in the
  `importer.less` file.
