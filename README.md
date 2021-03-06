# ember-strong-attrs

`ember-strong-attrs` is an addon that facilitates the declaration of
`Ember.Component` required and optional attributes. It extends
`Ember.Component` and provides [ES7 Decorators][decorators] to declare
**required** and **optional** attributes.

## Caveats

- Experimental. This is alpha software, and we think it's a *cool* idea but not sure
  if it's a *good* idea yet.
- You need to enable [ES7 Decorators][decorators] in Babel.
- [JSHint does not support ES7 Decorators at the moment][jshint-no-decorators] so you
  will get JSHint errors like this: ` Unexpected '@'.`. To avoid this, you can tell
  jshint to ignore your decorators for now, as shown in the examples below.
- Your `Ember.Component`s need to be ES6 classes so that the ES7 Decorators can
  decorate them.

## Setup

1. Install the addon

  ```
  ember install ember-strong-attrs
  ```

2. Update your `ember-cli-build.js` to enable Babel's ES7 Decorators

  ```js
  /* global require, module */
  var EmberApp = require('ember-cli/lib/broccoli/ember-app');

  module.exports = function(defaults) {
    defaults.babel = {
      optional: ['es7.decorators']
    };

    var app = new EmberApp(defaults, {});

    return app.toTree();
  };
  ```

3. Update the components you want to declare required/option attributes on to
   use [ES6 Classes syntax][classes].

  Given the following `Ember.Component` definition:

  ```js
  import Ember from 'ember';

  export default Ember.Component.extend({
    // ... your methods and props
  });
  ```

  You will get the following using [ES6 Classes syntax][classes]

  ```js
  import Ember from 'ember';

  export default class extends Ember.Component.extend({ // Note the class keyword
    // ... your methods and props
  }) { } // Don't forget the trailing { } and the removal of the semicolon
  ```

4. Import the decorators into your component file.

  ```js
  import { requiredAttr, optionalAttr } from 'ember-strong-attrs';
  ```

## API

### Supported Attribute types

The `attrType` argument can be the following classes:

- `String`
- `Number`
- `Date`
- `Function`
- `YouCustomClass`

### Decorators

`ember-strong-attrs` exposes two decorators:

- `@requiredAttr(attrName, attrType)`
- `@optionalAttr(attrName, attrType)`

Example:

```js
import Ember from 'ember';
import { requiredAttr, optionalAttr } from 'ember-strong-attrs';
import Person from '../models/person';

// Note the lack of semicolons behind the decorators
/* jshint ignore: start */
@requiredAttr('myRequiredAttr', String)
@optionalAttr('myStringAttr', String)
@optionalAttr('myPersonAttr', Person)
/* jshint ignore: end */
export default class extends Ember.Component.extend({
  // ... your methods and props
}) { }
```

### ES6 Compatible

`ember-strong-attr` exposes one function to declare strong attributes on
`Ember.Component`

- `declareStrongAttrs(attrsFunc, component)`, which returns the modified `component` that was passed in.

Example:

```js
import Ember from 'ember';
import { declareStrongAttrs } from 'ember-strong-attrs';
import Person from '../models/person';

export default declareStrongAttrs(function() {
  this.requiredAttr('myRequiredAttr', String);
  this.optionalAttr('myOptionalAttr', String);
  this.requiredAttr('myPersonAttr', Person);
}, Ember.Component.extend({
  // ... your methods and props
}));
```

[decorators]:https://github.com/wycats/javascript-decorators
[jshint-no-decorators]:http://jshint.com/blog/new-lang-features/
[classes]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes
