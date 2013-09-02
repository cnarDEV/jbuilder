# Jbuilder for Node.js

## TODO

* NPM release
* History
* Readme
    * Installation
    * Description

## Usage

```javascript
var jbuilder = require('./');

var user = {
  admin: true,
  name: 'Foo',
  profile: {
    imagePath: '/0815.jpg',

    chuck: {
      name: 'Norris'
    }
  }
};

var product = {
  price: 12.99,
  weight: '1kg',
  name: 'Awesome thing',
  currency: 'Euro',
  available: true
};

var output = jbuilder.encode(function(json) {
  // Set values
  json.set('name', user.name);

  // Conditions
  if (user.admin) {
    json.set('secretLink', 'http://...');
  }

  // Set second level values
  json.set('profile', function(json) {
    json.set('imagePath', user.profile.imagePath);

    // Set thrid level values
    json.set('chuck', function(json) {
      json.set('name', user.profile.chuck);
    });
  });

  // Ignore falsy values
  json.set('admin', false); // Present
  json.ignoreFalsy = true;

  var value = false;
  json.set('value', value); // Not present

  // Extract values
  json.set('product', function(json) {
    json.extract(product, 'price', 'name', 'weight');
  });

  // Turns object into array
  json.set('orders', function(json) {
    json.child(function(json) {
      json.set('id', 1);
    });
    json.child(function(json) {
      json.set('id', 2);
    });
  });
})

console.log(output);
// {"name":"Foo","secretLink":"http://...","profile":{"imagePath":"/0815.jpg","chuck":{"name":{"name":"Norris"}}},"admin":false,"product":{"price":12.99,"name":"Awesome thing","weight":"1kg"},"orders":[{"id":1},{"id":2}]}
```

## License

(The MIT License)

Copyright (c) 2013 Mario Behrendt info@mario-behrendt.de

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.