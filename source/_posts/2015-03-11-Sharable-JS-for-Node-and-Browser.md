title: "Shareable Code for Node and Browser"
date: 2015-03-11 23:00:07
tags: [programming, node, js, javascript]
---
I had a simple problem along the way of exploring Node.js, which involves writing modules that work for both server and client sides. Just like many programmers nowadays, I google'd out [a solution](https://caolanmcmahon.com/posts/writing_for_node_and_the_browser/) (which was written by the person who authored [`async`](https://github.com/caolan/async), so I guess I need to thank him at least twice).

However this doesn't solve all my problem since I am way too lazy to get rid of all those amazing open source libraries. <!-- more -->And:

> Of course, the browser doesn't support many other node features like require(), so you'll need to test that your code is suitable for use in the browser first.
> -- _Caolan McMahon_

Realizing dependencies can be passed into the exposed module as its arguments, I have finally landed a solution that is simple (for me) to grasp.

Say I needed to have a shared utility module for both Node.js and browser to use, one that rely on underscore.js and moment.js, which fortunately have fairly consistent interfaces for both platforms.

```javascript shared-utils.js
  // my module that relies on underscore.js and moment.js
  var utils = function(_, moment) {
    // private stuff
    var getNow = function(utcOffset) {
      return moment.utc().utcOffset(utcOffset || 0);
    };
    // ... more private stuff ...

    // public stuff
    return {
      sum: function(values) {
        return _.reduce(values, function(memo, num) {
          return memo + num;
        }, 0);
      },
      dateTimeRange: function(past, unit, utcOffset) {
        var dates = [];
        var now = getNow(utcOffset || 0);

        for (var i = 0; i < past; i++) {
          dates.push(moment(now).subtract(i, unit || 'day'));
        }

        return dates;
      },
      // ... more public stuff ...
    };
  };  // but NOT actually "self-excuting" this yet

  // "sync" the behavior of node require() and browser <script> tag
  if (typeof module !== 'undefined' && module.exports) {
    // we're on node.js, where we have the sweet module system to support exports and require
    module.exports = utils(
      require('underscore'),
      require('moment')
    );  // given that both packages are installed
  } else {
    // we're on browser, so let's "call" the defered self-executing function
    utils = utils(
      _,
      moment
    );  // similarly, given that both libraries are included already
  }
```

So now I could use this sweet little module in node:

```javascript app.js
  // expose public directory to client side
  // here I'm using express
  var express = require('express');
  var app = express();
  var server = require('http').createServer(app);
  app.use(express.static(__dirname + '/public'));

  // test it out on server side
  var utils = require('./public/js/shared-utils');

  console.log(utils.sum([1, 2, 3, 4, 5, 6, 7]));
```

and in browser:

```html index.html
  <script src="js/underscore.min.js"></script>
  <script src="js/moment.min.js"></script>
  <script src="js/shared-utils.js"></script>
  <script>
    console.log(utils.sum([1, 2, 3, 4, 5, 6, 7]));
  </script>
```

For now, I am satisfied.
