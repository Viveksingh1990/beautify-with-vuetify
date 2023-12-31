Directives
==========

Introduction
------------

* * *

One of the great things about Vue.js is that it provides us with directives such as:

*   v-if / v-show: for toggling the display of an element
*   v-on: for listening to events on an element

And while these built-in directives cover many common use cases, the Vuetify team has included some useful directives in order to enhance your development workflow.

In this lesson, we will be learning about:

1.  What Vuetify directives are
2.  Exploring the v-intersect in depth

Let’s get started!

Vuetify Directives
------------------

* * *

To get started using Vuetify directives, there’s no better place to start than the docs. On the [docs home page](https://vuetifyjs.com/en/getting-started/quick-start), you can find the Directives dropdown which contains a comprehensive list of all of the current directives in the latest version of Vuetify.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994157668_01-directives-docs.png?alt=media&token=cf650734-4e48-4498-b8f1-6abdb02ea49e](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994157668_01-directives-docs.png?alt=media&token=cf650734-4e48-4498-b8f1-6abdb02ea49e)

For our first directive, let’s take a look at the [resizing directive](https://vuetifyjs.com/en/directives/resizing):

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994359573_02-resize-directive.opt.gif?alt=media&token=ed8cdfde-fdb5-43d6-a7e7-587327cc7dbf](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994359573_02-resize-directive.opt.gif?alt=media&token=ed8cdfde-fdb5-43d6-a7e7-587327cc7dbf)

The resize directive allows you to detect when the window resizes so that you can respond accordingly. And using it is as simple as applying `v-resize` to an element with the callback function defined as the value.

    <template>
      <div v-resize="onResize">
        <!-- content -->
      </div>
    </template>
    

    <script>
    export default {
      methods: {
        onResize() {
          console.log('This window size is changing!')
        }
      }
    }
    </script>
    

Another fun directive is the ripple directive, which allows you to add a ripple animation to an HTML element in order to make it easier for users to detect when they interact with an element (i.e., clicking).

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994366013_03-ripple-directive.opt.gif?alt=media&token=47cffd38-c8bf-478b-a960-6f753927f634](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994366013_03-ripple-directive.opt.gif?alt=media&token=47cffd38-c8bf-478b-a960-6f753927f634)

Using the ripple directive is even simpler since all you have to do it apply if on the desired HTML element.

    <template>
      <div v-ripple>
        <p>HTML element with v-ripple applied</p>
      </div>
    </template>
    

The Loading Problem
-------------------

* * *

One scenario that is often overlooked in the user experience is the loading state. Whether it’s communicating to users that something is happening rather than simply displaying a blank canvas, or whether it is delaying a component from loading because it hasn’t come into the viewport yet, it’s a critical part of most application’s user experience.

Oftentimes, we send more information than necessary to users.

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994370469_04-waste-webpage.opt.gif?alt=media&token=20cab50f-62ad-4a61-a35f-eeee9c42b557](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994370469_04-waste-webpage.opt.gif?alt=media&token=20cab50f-62ad-4a61-a35f-eeee9c42b557)

As a result, users have larger download size which slows down performance and creates a sub-optimal user experience. Wouldn’t it be great if we could detect when a user has hit a certain point on the web page (i.e., below the fold) and then trigger additional content to load?

To help you with this, Vuetify 2.1+ introduced the `[v-intersect` directive\]([https://vuetifyjs.com/en/directives/intersect](https://vuetifyjs.com/en/directives/intersect)).

The purpose `v-intersect` directive is to provide an easy way to detect whether elements are visible within the user’s viewport. This allows us to do things like delay loading of elements which improves the performance of our app since users are only downloading what they need.

In our Vuetify Dashboard app, let’s apply this to our Dashboard page. Assuming you’ve fetched the latest from the `Lesson-8-BEGIN` branch, you’ll notice that I’ve added more content to the page in order to give the page a longer scroll range.

📄**src/views/Dashboard.vue**

    <template>
      <v-container>
        <!-- More content above -->
        <v-row>
          <v-col cols="12" md="8">
            <EmployeesTable :employees="employees" @select-employee="setEmployee" />
          </v-col>
          <v-col cols="12" md="4">
            <EventTimeline :timeline="timeline" />
          </v-col>
        </v-row>
      
        <v-row id="below-the-fold">
          <v-col cols="12" md="8">
            <EmployeesTable :employees="employees" @select-employee="setEmployee" />
          </v-col>
          <v-col cols="12" md="4">
            <EventTimeline :timeline="timeline" />
          </v-col>
        </v-row>
      
        <v-row id="more-content">
          <v-col>
            <v-skeleton-loader
              ref="skeleton"
              type="table"
              class="mx-auto"
            ></v-skeleton-loader>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

You might also notice a new component that is being used at the very bottom:

![https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994272764_05-skeleton-loader.opt.png?alt=media&token=4713ea03-a54b-4200-9a6f-dd8810d96d47](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1575994272764_05-skeleton-loader.opt.png?alt=media&token=4713ea03-a54b-4200-9a6f-dd8810d96d47)

This is the new [skeleton loader component](https://vuetifyjs.com/en/components/skeleton-loaders) which Vuetify introduced in v2.1+. This allows you to scaffold a contextual loading state for users, which is much more useful to users than a generic spinner.

However, we only want this to be visible once the user scrolls past the `#below-the-fold` element. So, let’s start by configuring our skeleton loader to only appear based on a reactive data property: `loadNewContent`.

📄**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
    
        <!-- Hide content for easier readability -->
    
        <v-row id="below-the-fold">
          <v-col cols="12" md="8">
            <EmployeesTable :employees="employees" @select-employee="setEmployee" />
          </v-col>
          <v-col cols="12" md="4">
            <EventTimeline :timeline="timeline" />
          </v-col>
        </v-row>
    
        <v-row v-if="loadNewContent" id="more-content">
          <v-col>
            <v-skeleton-loader
              ref="skeleton"
              type="table"
              class="mx-auto"
            ></v-skeleton-loader>
          </v-col>
        </v-row>
    
        <!-- Hide snackbar for easier readability -->
      </v-container>
    </template>
    

    <script>
    export default {
      data() {
        return {
          loadNewContent: false,
          ...
        }
      },
      ...
    }
    </script>
    

Now we need to dynamically toggle the skeleton loader based on whether the `#below-the-fold` element is intersected on the page. As mentioned before, we will use the `v-intersect` directive to help us with this. To get started, we will apply the `v-intersect` directive on our element and pass it a method named `showMoreContent`.

📄**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
    
        <!-- Hide content for easier readability -->
    
        <v-row id="below-the-fold" v-intersect="showMoreContent>
          <v-col cols="12" md="8">
            <EmployeesTable :employees="employees" @select-employee="setEmployee" />
          </v-col>
          <v-col cols="12" md="4">
            <EventTimeline :timeline="timeline" />
          </v-col>
        </v-row>
    
        <v-row v-if="loadNewContent" id="more-content">
          <v-col>
            <v-skeleton-loader
              ref="skeleton"
              type="table"
              class="mx-auto"
            ></v-skeleton-loader>
          </v-col>
        </v-row>
    
        <!-- Hide snackbar for easier readability -->
      </v-container>
    </template>
    

    <script>
    export default {
      data() {
        return {
          loadNewContent: false,
          ...
        }
      },
      methods: {
        showMoreContent(entries) { },
        ...
      },
      ...
    }
    </script>
    

You might have noticed that I’m including an `entries` argument to the function. This is because underneath the hood, `v-intersect` utilizes the standard [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API), which provides an array of elements being observed as one of the default parameters. And for each observer entry, it has a property called `isIntersecting` which returns a boolean based on whether the observed entry is in currently intersecting or not.

As a result, making our skeleton loader component load dynamically is as simple as reassigning the `loadNewContent` data property to the `isIntersecting` property for our entry.

📄**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
    
        <!-- Hide content for easier readability -->
    
        <v-row id="below-the-fold" v-intersect="showMoreContent>
          <v-col cols="12" md="8">
            <EmployeesTable :employees="employees" @select-employee="setEmployee" />
          </v-col>
          <v-col cols="12" md="4">
            <EventTimeline :timeline="timeline" />
          </v-col>
        </v-row>
    
        <v-row v-if="loadNewContent" id="more-content">
          <v-col>
            <v-skeleton-loader
              ref="skeleton"
              type="table"
              class="mx-auto"
            ></v-skeleton-loader>
          </v-col>
        </v-row>
    
        <!-- Hide snackbar for easier readability -->
      </v-container>
    </template>
    

    <script>
    export default {
      data() {
        return {
          loadNewContent: false,
          ...
        }
      },
      methods: {
        showMoreContent(entries) {
          this.loadNewContent = entries[0].isIntersecting
        },
        ...
      },
      ...
    }
    </script>
    

And with that, our skeleton element component only loads once the user goes past the `#below-the-fold` row! 🎉

To learn about other useful components related to the Intersection API, be sure to check out the `v-lazy` and `v-img` component!

As a small word of caution, you will need a polyfill for IE11 and Safari. For more details and how it all works under hood, be sure to check out the browser compatibility table in the official [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) docs.

Other Directives to Know About
------------------------------

* * *

Before we wrap up, there are a few more directives that you should know about:

*   [Mutation observer](https://vuetifyjs.com/en/directives/mutate) - Uses the Mutation Observer API to detect when elements are updated on the page. Particularly useful if you need to detect change on elements that are not within Vue’s control
*   [Scrolling](https://vuetifyjs.com/en/directives/scrolling) - For detecting when the window or a specific element is being scrolled on
*   [Touch support](https://vuetifyjs.com/en/directives/touch-support) - For detecting swipe gestures

Conclusion
----------

* * *

In this lesson, we learned about various directives in the Vuetify library, while also taking a closer look at the `v-intersect` directive. For more information on how to use each directive, be sure to check out the docs. In addition, don’t forget that this is not a fixed list! The team is always looking for ways to improve the developer experience and will be updating these as time goes on.

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-8-BEGIN)
    
*   [Finished Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-8-FINISH)
