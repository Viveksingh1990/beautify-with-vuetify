Layouts: Responsive Design
==========================

Introduction
------------

* * *

In the last lesson, we learned how to layout our Dashboard page on desktop devices; but we still need to learn how to handle other device sizes.

![Our Dashboard page on a mobile sized browser. Looks rather cramped doesn't it?](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1568690100851_01-mobile-issues.png?alt=media&token=fd74bf4e-7fd0-4813-9489-6147136ef662)

As you can see in the screen capture above, our Dashboard page isn’t so great when viewing on smaller devices. So it’s time that we learned how to support other device sizes by creating responsive designs with Vuetify.

In this lesson, we will be learning about:

*   What Vuetify’s breakpoints are
*   How to create responsive layouts with grid components
*   How to programmatically use Vuetify breakpoints to toggle styles and/or functionality based on a user’s device size.

### What are Vuetify breakpoints?

* * *

Like any responsive design system, Vuetify has breakpoints that allow you to control the layout of your application depending on the size of the window and/or device. Out of the box, there are five predefined [breakpoints in Vuetify](https://vuetifyjs.com/en/customization/breakpoints) which allow you to easily designate behavior and styles in Vuetify.

*   Extra small (`xs`) - For small devices like mobile devices
*   Small (`sm`) - For small to medium tablets
*   Medium (`md`) - For large tablet to desktop
*   Large (`lg`) - For desktop
*   Extra Large (`xl`) - For 4k and ultra-wide devices

Be sure to pay special attention to the two letter codes for each breakpoint, as they are used throughout Vuetify to refer to the various breakpoints.

And for those wondering, you can even define your own breakpoints if needed. To learn more, check out the [Breakpoint Usage docs](https://vuetifyjs.com/en/customization/breakpoints#usage) for more information.

So for our Dashboard page, we’ll need to fulfill the following requirements:

1.  On mobile devices, SaleGraphs will be 100% wide
2.  On tablet devices and larger, SalesGraph will be 33.33% wide
3.  On desktop devices and larger, the Snackbar will be positioned to the left

### Building mobile first with Vuetify

* * *

When it comes to applying our breakpoints with our app, the best way to approach this with Vuetify is **mobile first**. In other words, we start with the smallest device as a base layout and layer on as we go. Let’s take the SalesGraph component as an example.

When we defined the layout for `SalesGraph` components with the `cols` property, the key thing to understand here is that it defines the default layout at the smallest device. In other words, it will be overridden by any breakpoint defined.

Let’s start by setting our SalesGraphs to be 100% wide by default.

**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
    
        <v-row>
          <v-col v-for="sale in sales" :key="`${sale.title}`" cols="12">
            <SalesGraph :sale="sale" />
          </v-col>
        </v-row>
    
        <v-row>
          <v-col
            v-for="statistic in statistics"
            :key="`${statistic.title}`"
          >
            <StatisticCard :statistic="statistic" />
          </v-col>
        </v-row>
    
        <v-row>
          <v-col cols="8">
            <EmployeesTable :employees="employees" @select-employee="setEmployee" />
          </v-col>
          <v-col cols="4">
            <EventTimeline :timeline="timeline" />
          </v-col>
        </v-row>
    
        <v-snackbar v-model="snackbar">
          You have selected {{ selectedEmployee.name }},
          {{ selectedEmployee.title }}
          <v-btn color="pink" text @click="snackbar = false">
            Close
          </v-btn>
        </v-snackbar>
      </v-container>
    </template>
    

    <script>
    export default {
      // Excluded for brevity
    }
    </script>
    

![How our SalesGraph looks across devices starting from mobile](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1568690104731_02-sales-graph-full-width.gif?alt=media&token=8994f0f4-9f1b-4a79-972e-6741106e304f)

While our SalesGraphs now look great on mobile, it looks like our three column layout for desktop is gone! 😱 No need to fret though, this is where breakpoints come in. All we need to do is set the columns when the devices are tablet size or larger. And we will do this with the `md` breakpoint.

**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
    
        <v-row>
          <v-col v-for="sale in sales" :key="`${sale.title}`" cols="12" md="4">
            <SalesGraph :sale="sale" />
          </v-col>
        </v-row>
    
        <v-row>
          <v-col
            v-for="statistic in statistics"
            :key="`${statistic.title}`"
          >
            <StatisticCard :statistic="statistic" />
          </v-col>
        </v-row>
    
        <v-row>
          <v-col cols="8">
            <EmployeesTable :employees="employees" @select-employee="setEmployee" />
          </v-col>
          <v-col cols="4">
            <EventTimeline :timeline="timeline" />
          </v-col>
        </v-row>
    
        <v-snackbar v-model="snackbar">
          You have selected {{ selectedEmployee.name }},
          {{ selectedEmployee.title }}
          <v-btn color="pink" text @click="snackbar = false">
            Close
          </v-btn>
        </v-snackbar>
      </v-container>
    </template>
    

    <script>
    export default {
      // Excluded for brevity
    }
    </script>
    

![Screenshot of how a responsive layout for SalesGraph](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1568690108812_03-sales-graph-responsive.gif?alt=media&token=f555341c-cd87-40ff-b0ee-6664dc2c3353)

And with that, our SalesGraph looks great!

Using breakpoints programatically
---------------------------------

* * *

Before we can wrap up this lessons, we still have one more requirement to fulfill though:

*   On desktop devices and larger, the Snackbar will be positioned to the left

In the [Snackbar API docs](https://vuetifyjs.com/en/components/snackbars#api), we find that there is a `left` prop that allows us to position our Snackbar component to the left as desired and takes a value of true or false. If we were to write the code required to detect a user’s device size, it might look something as follows:

**<!— Theoretical Vue SFC Component —>**

    <script>
      export default {
        data: () => ({
          // Create a reactive data property to track
          // whether the user is on a mobile device
          isMobile: false,
        }),
    
        beforeDestroy () {
          if (typeof window !== 'undefined') {
            // Remove the resize event when the component is destroyed
            window.removeEventListener('resize', this.onResize, { passive: true })
          }
        },
    
        mounted () {
          // Check whether the user's device is mobile or not
          this.onResize()
          // Create resize event to check for mobile device
          window.addEventListener('resize', this.onResize, { passive: true })
        },
    
        methods: {
          onResize () {
            // Set reactive data property to true
            // if device is less than 600 pixels
            this.isMobile = window.innerWidth < 600
          },
        },
      }
    </script>
    

That’s quite a lot of boilerplate code just to check if a user is on a device of a certain screen size. 😫 Wouldn’t it be great if we could have an easy to call function that did all this work for us? Well we are in luck, because Vuetify has done just that with `$vuetify.breakpoinnt`.

One of the properties available to you when calling `$vuetify` is `breakpoint`, which gives you access to all of the breakpoints currently available on that instance of Vuetify. Here are some examples:

*   `md` - Returns a Boolean for whether the device is between 960px and 1264px\*
*   `mdAndUp` - Returns a Boolean for whether the device is greater than or equal to 960px
*   `mdAndDown` - Retursn a Boolean for whether the device is less than or equal to 960px

Since we need to only set the `left` prop for desktop devices or larger, we need to track `lgAndUp`. So our code should look something like this:

**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
    
        <v-row>
          <v-col v-for="sale in sales" :key="`${sale.title}`" cols="12" md="4">
            <SalesGraph :sale="sale" />
          </v-col>
        </v-row>
    
        <v-row>
          <v-col
            v-for="statistic in statistics"
            :key="`${statistic.title}`"
          >
            <StatisticCard :statistic="statistic" />
          </v-col>
        </v-row>
    
        <v-row>
          <v-col cols="8">
            <EmployeesTable :employees="employees" @select-employee="setEmployee" />
          </v-col>
          <v-col cols="4">
            <EventTimeline :timeline="timeline" />
          </v-col>
        </v-row>
    
        <v-snackbar v-model="snackbar" :left="$vuetify.breakpoint.lgAndUp">
          You have selected {{ selectedEmployee.name }},
          {{ selectedEmployee.title }}
          <v-btn color="pink" text @click="snackbar = false">
            Close
          </v-btn>
        </v-snackbar>
      </v-container>
    </template>
    

    <script>
    export default {
      // Excluded for brevity
    }
    </script>
    

![Our Dashboard page with responsive SalesGraph layout and proper Snackbar positioning](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1568690111581_04-responsive-part-1-complete.gif?alt=media&token=13d20583-fcb3-40d5-a807-693e23a28e7a)

And with that, we have fulfilled the first part of the responnsive design requirements. Great work! 🎉

Conclusion
----------

* * *

In this lesson, we have learned about:

*   What Vuetify breakpoints are
*   How to create responsive layouts with grid components
*   How to programmatically use Vuetify breakpoints

For this lesson’s coding challenge, here are the remaining responsive design requirements:

1.  On mobile devices:
    *   All components should have a width of 100%
2.  On tablet devices and larger:
    *   Each StatisticCard should have a width of 50%
    *   EmployeesTable should have a width of 66.67%
    *   EventTimeline should have a width of 33.33%
3.  On desktop devices and larger:
    *   Each StatisticCard should have a width of 25% on large devices and up

Congratulations on finishing this part of the course! You have acquired the skills and knowledge needed to create beautiful layouts on devices on all sizes. Good luck on the code challenge and see you in the next lesson!

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-5-BEGIN)
    
*   [Finished Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-5-FINISH)
