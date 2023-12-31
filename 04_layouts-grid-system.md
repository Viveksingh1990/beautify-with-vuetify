Layouts: Grid System
====================

In the last lesson, we learned about how components work and how we can leverage them in order to maximize our productivity. It’s time we learned about another critical aspect of any application’s design and user experience: layout.

In this lesson, we will be learning about:

*   How to use grid system components
*   How the grid system works
*   How to integrate the grid system into our dashboard app

Fast Forward to More Content
----------------------------

* * *

After Lesson 3, our dashboard didn’t require much layout since it only contained a single component. In order to simulate a more realistic dashboard, we’re going to provide some filler content to our dashboard page.

If you are coding along, please make sure to clone the [repo on the Lesson-4-BEGIN branch](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-4-BEGIN) to have the correct code.

When we start up our application, we should see the following:

![Preview of updated Dashboard app](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1567438948036_01-intro-dashboard.png?alt=media&token=a67d87a7-8359-473f-b58a-aa3f2d9ad88d)

### What has changed?

* * *

In the [Component Pt. 2](https://www.vuemastery.com/courses/beautify-with-vuetify/components-part-2), we only had a single data table component. In this updated Dashboard, I’ve gone ahead and updated our code with new and refactored components to simplify our Dashboard page. If you’re curious, feel free to take a look at how they work, but it is not necessary for this lesson.

*   The existing data table component has been refactored to `EmployeesTable.vue`
*   A new `EventTimeline.vue` component which makes use of **[Timelines](https://vuetifyjs.com/en/components/timelines)** which is a component used for stylistically displaying chronological information
*   A new `SalesGraph.vue` component which makes use of **[Sparklines](https://vuetifyjs.com/en/components/sparklines)**, which is a component for generating simple graphs
*   A new `StatisticCard.vue` which uses the **[Card](https://vuetifyjs.com/en/components/cards#cards)** component we already know how to use
*   Data sources for components
    *   `src/data/employees.json` - A list of employees that populates the EmployeesTable component
    *   `src/data/sales.json` - A list of sales data that will populate the graphs in the SalesGraph component
    *   `src/data/statistics.json` - A list of statistics used to populate the StatisticCard component
    *   `src/data/timeline.json` - A list of events used to populate the EventTimeline component

While there are two new components being used, the knowledge you acquired from [Components - Pt. 1](https://www.vuemastery.com/courses/beautify-with-vuetify/components-part-1) and [Component Pt. 2](https://www.vuemastery.com/courses/beautify-with-vuetify/components-part-2) have made you more than capable to explore how they work on your own.

As you can see, while there is a lot of useful information here for users, we have some work to do before it’s a good user experience. So let’s start with Vuetify’s Grid system!

Vuetify’s Grid System
---------------------

* * *

The primary foundation for layout with Vuetify is its grid system. Inspired by [Bootstrap’s grid system](https://getbootstrap.com/docs/4.0/layout/grid/), Vuetify’s grid system is built with flexbox and consists of four primary components:

*   **Container** (`v-container`) - This is the base wrapper for creating any grid layout on your page. By default, it will provide default max-width behavior to ensure your content is accessible at larger device sizes
*   **Row** (`v-row`) - This is the wrapper component around columns and should be a direct child of `v-container`
*   **Column** (`v-col`) - The wrapper around content, which must be a direct child of `v-row`
*   **Spacer** (`v-spacer`) - A useful component for filling available space or making space between two components

These comprise of the fundamental building blocks for laying out and aligning content on the page. So to get started, let’s start by switching out the wrapper div on Dashboard for `v-container`.

**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
        <!-- Excluding the rest for brevity -->
      </v-container>
    </template>
    
    <script>
    export default {
      // Excluded for brevity
    }
    </script>
    

![Screenshot of root container when wrapped with v-container](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1567438949703_02-root-container-wrapper.png?alt=media&token=3a9a943f-3808-4989-bda1-e98ad848b874)

If you are on a larger screen size, you should see that our content is now centered on the page!

### Layout with columns of equal sizes

* * *

Next, we need to layout our SalesGraph and StatisticCard components. The requirements are as follows for desktop devices and larger:

*   SalesGraphs are displayed in a single row
*   SalesGraphs should be 33.33% width
*   StatisticCard are displayed in a single row
*   StatisticCard should be 25% width

So, we start by wrapping our content in `v-row` and give each individual SalesGraph component its own column. We then move the `v-for` directive onto the `v-col` to ensure that we are generating new columns for each new SalesGraph.

**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
        <v-row>
          <v-col v-for="(graph, index) in sales" :key="`${graph.title}-${index}`">
            <SalesGraph :sales="sales" />
          </v-col>
        </v-row>
        <!-- Excluding the rest for brevity -->
      </v-container>
    </template>
    
    <script>
    export default {
      // Excluded for brevity
    }
    </script>
    

![Screenshot of SalesGraph layout](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1567438950722_03-sales-graphs-layout.png?alt=media&token=2035b6e4-1c50-4876-85ea-94f813bd5057)

Since Vuetify is using Flexbox, we don’t need any additonal configuration required since the space is automatically distributed between each column. That was easy!

Let’s go ahead and layout our StatisticCards!

**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
    
        <v-row>
          <v-col v-for="sale in sales" :key="`${sale.title}`">
            <SalesGraph :sale="sale" />
          </v-col>
        </v-row>
    
        <v-row>
          <v-col v-for="statistic in statistics" :key="`${statistic.title}`">
            <StatisticCard :statistic="statistic" />
          </v-col>
        </v-row>
    
        <!-- Excluding the rest for brevity -->
      </v-container>
    </template>
    
    <script>
    export default {
      // Excluded for brevity
    }
    </script>
    

![Screenshot of StatisticCard laid out on desktop](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1567438952064_04-statistic-card-layout.png?alt=media&token=3f467495-7225-451f-bee5-7141f20d5309)

With that, you now know how to layout content in evenly sized columns!

### Layout with columns of different sizes

* * *

Next, our designer would like us to layout our data table component and timeline component with the following requirement on desktop devices and larger

*   EmployeesTable and EventTimeline will exist in the same row
*   EmployeesTable should be two-third (i.e., 66.67%) of the row width
*   EventTimeline should be a third (i.e., 33.33%) of the row width

Rather than write custom CSS, Vuetify makes it easy for us to define column widths. Vuetify is built on a 12 column grid system. As a result, defining a `v-col` width is as simple as passing in a `cols` prop with the number of columns desired. Here are some sample calculations:

*   25% width - 3 columns / 12 max columns
*   50% width - 6 columns / 12 max columns
*   66.67% width - 8 columns / 12 max columns
*   100% width - 12 columns / 12 max columns

With that knowledge, let’s layout our `EmployeesTable` and `EventTimeline`!

**src/views/Dashboard.vue**

    <template>
      <v-container>
        <h1>Dashboard</h1>
    
        <v-row>
          <v-col v-for="sale in sales" :key="`${sale.title}`">
            <SalesGraph :sale="sale" />
          </v-col>
        </v-row>
    
        <v-row>
          <v-col v-for="statistic in statistics" :key="`${statistic.title}`">
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
    

![Screenshot of uneven column widths for EmployeesTable and EventTimeline](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1567438953756_05-uneven-layout-finish.png?alt=media&token=26fe4bda-a0a4-4b52-8dc9-23414463a839g)

And with that, our Dashboard app is looking much better! 🎉

Conclusion
----------

* * *

In this lesson, we have learned about:

*   Grid system components
*   How the grid system works
*   How to integrate the grid system into our Dashboard app

Now, you may be wondering, “What about the rest of the devices? Didn’t we only layout the desktop?” And you would be absolutely right. That’s why in the next lesson, we will take a look at how to handle responsive design with Vuetify. See you there!

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-4-BEGIN)
    
*   [Finished Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-4-FINISH)
