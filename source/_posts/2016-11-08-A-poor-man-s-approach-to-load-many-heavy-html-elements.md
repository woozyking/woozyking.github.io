title: A poor man's approach to load many heavy HTML elements
date: 2016-11-08 20:54:53
tags: [dota, javascript, vue.js, chart.js]
---
![Poor Man's Shield](https://images.akamai.steamusercontent.com/ugc/71248569151574222/0E1D5FBE20A79A1633B3D35B0BC086E4929F1B79/)

If you've played [Dota](https://blog.dota2.com), then you'd know an item called [Poor Man's Shield](https://dota2.gamepedia.com/Poor_Man%27s_Shield). Without getting too much into the meta and expose my laughable understanding of the game, the item is mostly about cost efficient way to deal with relatively tough situations in the early game, without the late game resources to go for a more luxurious option right away (except when you can stomp in early game against noobs like me).

In web development, there can be tough situations as well. One situation I encountered recently was to load potentially thousands of items each with multiple charts on a single page, in a nutshell:

![So Many Charts](/images/screenshots/many_elements_with_charts.png)

In this post, I'm going to walk through my steps of developing the poor man's approach to handle the tough situation. For examples below, we are going to utilize `Vue.js v2` as the data binding layer, and `Chart.js v2` for chart.

### Be Naive

Starting with the most naive approach:

```javascript
var app = new Vue({
  //... other configurations
  components: {
    chart: {
      template: '<canvas :height="height"></canvas>',
      props: ['height', 'data', 'options', 'type'],
      methods: {
        render: function(el) {
          var self = this;
          self.chart = new Chart((el || self.$el).getContext('2d'), {
            type: self.type || 'line',
            data: self.data || {
              labels: [],
              datasets: []
            },
            options: self.options || {}
          });
        }
      },
      mounted: function() {
        var self = this;
        // initialize the chart
        self.render();
      }
    }
  }
});
```
([fully working jsfiddle](https://jsfiddle.net/woozyking/vd2psadj/))

This works. However it becomes royally painful to load when you have so many items as the situation I described earlier. In fact, just a mere hundred of such items on a single page already causes unpleasant amount of time to render while showing a blank page, and blocking all user interactions except inviting end users to force close the browser/tab due to frustration (and probably never getting them back). Such horror is not tolerable on today's internet.

### Be Asynchronous

One way we can make this better is to explicitly instruct the component to asynchronously initialize the chart through the use of `setTimeout`:

```javascript
var app = new Vue({
  //... other configurations
  components: {
    chart: {
      //... same component configurations as above
      mounted: function() {
        var self = this;
        // asynchronously initialize the chart
        setTimeout(function() {
          self.render();
        }, 0);
      }
    }
  }
});
```
([fully working jsfiddle](https://jsfiddle.net/woozyking/pghb6znL/))

This approach effectively causes `Vue` to render the charts in the "next" update cycle. It makes the UI a bit less painful to use as there can be some "primer" contents rendered, which usually exhibit as a part of the parent elements that hold the chart component. However, this is not good enough, as the deferred update cycle would still block the whole UI. This is especially bad when the page offers many items in a list fashion that the users would want to either search with `CTRL/CMD + F` or scroll down for items that are out of the current view. Besides, it's very wasteful to render things that cannot even be seen yet.

### Be Lazy

This leads to a tried and proven technique many refer to as "lazy-loading". With so many years of such technique being matured, and thanks to the ever so successful open source community, one can easily pick a robust library that does it well while being delightfully simple to use. The following is an approach with one such library called [ScrollReveal](https://scrollrevealjs.org/):

```javascript
var sr = ScrollReveal();
var app = new Vue({
  //... other configurations
  components: {
    chart: {
      //... same component configurations as above
      mounted: function() {
        var self = this;
        sr.reveal(self.$el, {
          afterReveal: self.render
        });
      }
    }
  }
});
```
([fully working jsfiddle](https://jsfiddle.net/woozyking/7k02a01w/))

It's pretty amazing now, the component rendering get deferred until they are needed by the end users.

### Extra

After handling the situations for the sake of end users, let's make a final touch to give some options for the users of this component to have control over the render flow, for example:

```javascript
var sr = ScrollReveal();
var app = new Vue({
  //... other configurations
  components: {
    chart: {
      //... same component configurations as above
      props: [
        'height', 'data', 'options', 'type',
        'flow' // let's add a 'flow' parameter
      ],
      mounted: function() {
        var self = this;
        if (self.flow == 'sync') {
          self.render();
        } else if (self.flow == 'async') {
          setTimeout(function() {
            self.render();
          }, 0);
        } else {
          sr.reveal(self.$el, {
            afterReveal: self.render
          });
        }
      }
    }
  }
});
```

The `flow` parameter allows 3 modes, let's go by imaginary use cases:

- `sync` would be used when this chart component is nested under another component (which hopefully already applies some sort of render deferring optimization similar to what we've done thus far), __and__ requires the chart to visually appear at the same time as its parent component
- `async` is very similar to `sync`, except that we don't require the chart to visually appear at the exact same moment as its parent component. 
- The third and the default mode would be the self-contained optimal approach that we worked out from above.

There's room for more sophistication. One can wrap the above into an easy-to-use custom directive or wrapper component, or even directly make it into `Vue.js`, making it more of an "end game" approach. With some twist, this approach can also be adapted into other choices of data binding layer and chart library.

But there's always the option to employ the poor man's approach -- simple, effective, easy to understand and not too shabby to apply to any existing applications of any scale.
