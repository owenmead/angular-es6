# angular-es6
Messing Around with ES6 and Angular. Below is the code I came up with to have a sane way to define angular 'pieces' in ES6 classes. **Note:** I'm using webpack with `babel` and `ng-annotate` to do this:
```
loader: 'ng-annotate!babel',
```

### Sample Code:
```javascript
'use strict';

/* Don't use factory when adding Class objects
 * Factory is for returning abritrary objects/data.
 * Services invokes your class, which is what we want
 * -----------------------
 * The following will do it...
 *   but you've essentailly just made a service with extra typing...
 *     ... which is dumb
 *
 *    class MyFactory {
 *      constructor(NoteService) {
 *        console.log('MyFactory', NoteService);
 *      }
 *    }
 *    angular.module('app')
 *      .factory('MyFactory', ($injector) => $injector.instantiate(MyFactory));
 *
 *    // == More explicitly, but MUCH less DRY ====
 *    angular.module('app')
 *      .factory('MyFactory', (NoteService) => new MyFactory(NoteService));
 */

angular.module('app', []);


class NoteService {
  constructor() { // can inject ng-modules here
    this.value = 0;
  }

  moreValue() {
    this.value++;
  }
}
angular.module('app').service('NoteService', NoteService);


class NoteController {
  constructor(NoteService) {
    this.NoteService = NoteService;
    this.theData = 0;
  }

  setData() {
    this.theData++;
  }
}
angular.module('app').controller('NoteController', NoteController);


class NoteDirective {
  constructor($timeout) {
    this.restrict = 'E';
    this.template = require('./notes.html');
    this.controller = 'NoteController';
    this.controllerAs = 'ctrl';

    console.log($timeout);
  }
}
angular.module('app').directive('noteDirective', ($injector) => $injector.instantiate(NoteDirective));
```
