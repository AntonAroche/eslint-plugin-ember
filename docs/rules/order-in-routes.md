# order-in-routes

## Configuration

```js
ember/order-in-routes: [2, {
  order: [
    'service',
    'inherited-property',
    'property',
    'single-line-function',
    'multi-line-function',
    'beforeModel',
    'model',
    'afterModel',
    'serialize',
    'redirect',
    'activate',
    'setupController',
    'renderTemplate',
    'resetController',
    'deactivate',
    'actions',
    ['method', 'empty-method'],
  ]
}]
```

If you want some of properties to be treated equally in order you can group them into arrays, like so:

```js
order: [
  'service',
  ['inherited-property', 'property'],
  'model',
  [
    'beforeModel',
    'model',
    'afterModel',
    'serialize',
    'redirect',
    'activate',
    'setupController',
    'renderTemplate',
    'resetController',
    'deactivate'
  ],
  'actions',
  ['method', 'empty-method'],
]
```

### Custom Properties

If you would like to specify ordering for a property type that is not listed, you can use the custom property syntax `custom:myPropertyName` in the order list to specify where the property should go.

### Additional Properties

You can find the full list of properties [here](/lib/utils/property-order.js#L10).

## Description

You should write code grouped and ordered in this way:

1. Services
2. Default route's properties
3. Custom properties
4. beforeModel() hook
5. model() hook
6. afterModel() hook
7. Other lifecycle hooks in execution order (serialize, redirect, etc)
8. Actions
9. Custom / private methods

```javascript
const { Route, inject: { service }, get } = Ember;

export default Route.extend({
  // 1. Services
  currentUser: service(),

  // 2. Default route's properties
  queryParams: {
    sortBy: { refreshModel: true },
  },

  // 3. Custom properties
  customProp: 'test',

  // 4. beforeModel hook
  beforeModel() {
    if (!get(this, 'currentUser.isAdmin')) {
      this.transitionTo('index');
    }
  },

  // 5. model hook
  model() {
    return this.store.findAll('article');
  },

  // 6. afterModel hook
  afterModel(articles) {
    articles.forEach((article) => {
      article.set('foo', 'bar');
    });
  },

  // 7. Other route's methods
  setupController(controller) {
    controller.set('foo', 'bar');
  },

  // 8. All actions
  actions: {
    sneakyAction() {
      return this._secretMethod();
    },
  },

  // 9. Custom / private methods
  _secretMethod() {
    // custom secret method logic
  },
});
```
