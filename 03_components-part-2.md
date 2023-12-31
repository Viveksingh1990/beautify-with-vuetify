Components (Pt. 2)
==================

In the last lesson, Vuetify Components Part 1, we started learning how to use Vuetify components by adding a global header and footer. In this lesson, we are going to take our app to the next level with

*   multi-page navigation
*   take a deeper dive into components docs
*   learn how to use more complex component

Managing navigation with Vuetify is no different than any other Vue app since all we need is Vue Router. In this lesson, I’ll go through the basic steps to use Vue Router, but for an in-depth exploration of Vue Router, make sure to check out our [Real World Vue course](https://www.vuemastery.com/courses/real-world-vue-js/) where we teach you about Vue Router.

Let’s get started by opening up our project in our terminal.

Adding Navigation to Our App
----------------------------

* * *

Once we open up our project in the terminal, we will start by adding Vue Router into our app with Vue CLI by using the following command:

    vue add router
    

When prompted for history mode, we’ll go ahead and enable it in order to have standard URLs rather than hash URLs.

![Screenshot of terminal with Router History mode](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707020062_00-vue-router-history.png?alt=media&token=afa7942e-61b6-4dc1-9423-c7d5b0ab5a02)

Once it finishes installing, let’s fire up our local web server to see what it looks like with:

    yarn serve
    

![Screenshot of broke App.vue](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707020884_01-vue-router-breaks-home-page.png?alt=media&token=ce7b987b-8727-4e1a-afe3-c2ca3af19f5b)

😱 Oh no! It looks like everything has been wiped out! 😭

Well do not fear. Version control can save the day. So let’s dive into our Source Control tab in VS Code to see what happened.

![Screenshot of the diff of App.vue on VS Code](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707022179_02-vue-router-get-diff-on-app-vue.png?alt=media&token=97e9ecc3-84d1-4382-91ff-03de42cd60dc)

Based on the diff between the past and current `App.vue` file, it becomes clear that vue-router installation process overwrote the file with a boilerplate file. So all we have to do is revert the change by discarding the current changes.

![Screenshot of Discard change icon on VS Code](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707023927_03-vue-router-discard-changes.png?alt=media&token=44c1878b-2922-4362-85c1-a73d336f637d)

Once we discard the changes, everything is back to normal. 👍

Next, let’s go ahead and refactor our `App.vue` file so that our Login module lives in its own page. To do this, we will need to:

1.  Create a new file in the `src/views` directory called `Login.vue`
2.  Cut and paste the login module from `App.vue` into `Login.vue`
3.  Move the `showPassword` data property into `Login.vue`

**src/views/Login.vue**

    <template>
      <v-card width="400" class="mx-auto mt-5">
        <v-card-title class="pb-0">
          <h1 class="display-1">Login</h1>
        </v-card-title>
        <v-card-text>
          <v-form>
            <v-text-field 
              label="Username" 
              prepend-icon="mdi-account-circle"
            />
            <v-text-field 
              :type="showPassword ? 'text' : 'password'" 
              label="Password"
              prepend-icon="mdi-lock"
              :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
              @click:append="showPassword = !showPassword"
            />
          </v-form>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-btn color="success">Register</v-btn>
          <v-spacer></v-spacer>
          <v-btn color="info">Login</v-btn>
        </v-card-actions>
      </v-card>
    </template>
    

    <script>
    export default {
      name: 'Login',
      data() {
        return {
          showPassword: false
        }
      }
    }
    </script>
    

**src/App.vue**

    <template>
      <v-app>
        <v-app-bar app color="primary" dark>
          <v-toolbar-title>Vuetify Dashboard</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn text rounded>Home</v-btn>
          <v-btn text rounded>Login</v-btn>
        </v-app-bar>
        <v-content>
    
        </v-content>
        <v-footer color="primary lighten-1" padless>
          <v-layout justify-center wrap>
            <v-btn
              v-for="link in links"
              :key="link"
              color="white"
              text
              rounded
              class="my-2"
            >
              {{ link }}
            </v-btn>
            <v-flex primary lighten-2 py-4 text-center white--text xs12>
              {{ new Date().getFullYear() }} — <strong>Vuetify Dashboard</strong>
            </v-flex>
          </v-layout>
        </v-footer>
      </v-app>
    </template>
    

    <script>
    export default {
      name: 'App',
      data () {
        return {
          links: [
            'Home',
            'Login'
          ]
        }
      }
    }
    </script>
    

4.  Add a new route for the `/login` in `router.js`.

**src/router.js**

    import Vue from 'vue'
    import Router from 'vue-router'
    import Home from './views/Home.vue'
    
    Vue.use(Router)
    
    export default new Router({
      mode: 'history',
      base: process.env.BASE_URL,
      routes: [
        {
          path: '/',
          name: 'home',
          component: Home
        },
        {
          path: '/about',
          name: 'about',
          // route level code-splitting
          // this generates a separate chunk (about.[hash].js) for this route
          // which is lazy-loaded when the route is visited.
          component: () => import(/* webpackChunkName: "about" */ './views/About.vue')
        },
        {
          path: '/login',
          name: 'login',
          component: () => import('./views/Login.vue')
        }
      ]
    })
    

5.  Add a `router-view` element to `App.vue` so that the correct page is displayed

**src/App.vue**

    <template>
      <v-app>
        <v-app-bar app color="primary" dark>
          <v-toolbar-title>Vuetify Dashboard</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn text rounded>Home</v-btn>
          <v-btn text rounded>Login</v-btn>
        </v-app-bar>
        <v-content>
    
        </v-content>
        <v-footer color="primary lighten-1" padless>
          <v-layout justify-center wrap>
            <v-btn
              v-for="link in links"
              :key="link"
              color="white"
              text
              rounded
              class="my-2"
            >
              {{ link }}
            </v-btn>
            <v-flex primary lighten-2 py-4 text-center white--text xs12>
              {{ new Date().getFullYear() }} — <strong>Vuetify Dashboard</strong>
            </v-flex>
          </v-layout>
        </v-footer>
      </v-app>
    </template>
    

    <script>
    export default {
      name: 'App',
      data () {
        return {
          links: [
            'Home',
            'Login'
          ]
        }
      }
    }
    </script>
    

6.  Delete the Vue logo from `Home.vue` on line 3.

**src/views/Home.vue**

    <template>
      <div class="home">
        <HelloWorld msg="Welcome to Your Vue.js App"/>
      </div>
    </template>
    

    <script>
    // @ is an alias to /src
    import HelloWorld from '@/components/HelloWorld.vue'
    
    export default {
      name: 'home',
      components: {
        HelloWorld
      }
    }
    </script>
    

With this refactor, now our app has two pages!

**Home - `/`**

![Screenshot of Home Page](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707025541_04-home-page-screenshot.png?alt=media&token=f0413e1c-012f-4794-b61d-88dc05df31ca)

**Login -** `/login`

![Screennshot of Login Page](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707026677_05-login-page-screenshot.png?alt=media&token=8512d9db-40f6-40cf-9819-f323de3369c7)

While we have proper routes now, currently our Vuetify buttons in the global navigation and footer do not do anything. In addition, while these components may look like buttons visually, the final rendered HTML should change according to its semantic usage (i.e., an anchor tag for a link).

If we look at the [Button component API documentation](https://vuetifyjs.com/en/components/buttons#api), there are two different props worth noting:

*   `href` - Any `v-btn` configured with this prop will be rendered as a normal anchor element
*   `to` - This converts it to a `router-link` element, which means you can also use any properties defined from the [Vue Router API](https://router.vuejs.org/api/#router-link). Just what we are looking for!

Let’s go ahead and configure our `v-btn`s in our App Bar so that it can route to the pages we just setup.

**src/App.vue**

    <template>
      <v-app>
        <v-app-bar app color="primary" dark>
          <v-toolbar-title>Vuetify Dashboard</v-toolbar-title>
          <v-spacer></v-spacer>
          **** <v-btn text rounded to="/">Home</v-btn>
          <v-btn text rounded to="/login">Login</v-btn>
        </v-app-bar>
        <v-content>
          <router-view></router-view>
        </v-content>
        <v-footer color="primary lighten-1" padless>
          <v-layout justify-center wrap>
            <v-btn
              v-for="link in links"
              :key="link"
              color="white"
              text
              rounded
              class="my-2"
            >
              {{ link }}
            </v-btn>
            <v-flex primary lighten-2 py-4 text-center white--text xs12>
              {{ new Date().getFullYear() }} — <strong>Vuetify Dashboard</strong>
            </v-flex>
          </v-layout>
        </v-footer>
      </v-app>
    </template>
    

    <script>
    export default {
      name: 'App',
      data () {
        return {
          links: [
            'Home',
            'Login'
          ]
        }
      }
    }
    </script>
    

Now when you click on the buttons, they properly route you to the correct page. And as an added bonus, it even tracks what page the user is currently on and automatically applies the correct style. Sweet! 🎉

However, it looks like our Footer buttons are still not functional. In addition, any future navigation changes will require changing the code in two places. So rather than configuring the links twice, let’s refactor our code so that the App Bar buttons and Footer buttons use the same data property.

1.  Convert `links` data property to be an array of objects that has a `label` property for what is displayed to users and a `url` property for routing purposes
2.  Update the following propertes of `v-btn` in the footer:

*   Key
*   To
*   Displayed Text

3.  Update the `v-btn` in the App Bar to use the `v-for` directive to loop through the `links` data property and update the following properties:

*   Key
*   To
*   Displayed Text

**src/App.vue**

    <template>
      <v-app>
        <v-app-bar app color="primary" dark>
          <v-toolbar-title>Vuetify Dashboard</v-toolbar-title>
          <v-spacer></v-spacer>
          **<v-btn 
            v-for="link in links"
            :key="`${link.label}-header-link`"
            text 
            rounded 
            :to="link.url"
          >
            {{ link.label }}
          </v-btn>**
        </v-app-bar>
        <v-content>
          <router-view></router-view>
        </v-content>
        <v-footer color="primary lighten-1" padless>
          <v-layout justify-center wrap>
            **<v-btn
              v-for="link in links"
              :key="`${link.label}-footer-link`"
              color="white"
              text
              rounded
              class="my-2"
              :to="link.url"
            >
              {{ link.label }}
            </v-btn>**
            <v-flex primary lighten-2 py-4 text-center white--text xs12>
              {{ new Date().getFullYear() }} — <strong>Vuetify Dashboard</strong>
            </v-flex>
          </v-layout>
        </v-footer>
      </v-app>
    </template>
    

    <script>
    export default {
      name: 'App',
      data () {
        return {
          links: [
            {
              label: 'Home',
              url: '/'
            },
            {
              label: 'Login',
              url: '/login'
            }
          ]
        }
      }
    }
    </script>
    

Awesome! We could take this refactor a step further and create a component like `NavLink` to further simplify our code, but I’ll leave that one to you for extra credit.

Now that we have navigation working propertly in our app, it’s time to create the main page that our app desperately needs: the dashboard page!

Creating Our Dashboard Page
---------------------------

* * *

With a project named `vuetify-dashboard`, it’s about time that we created a dashboard page to give the app some life. To get us started, we need to:

1.  Create a new page `src/views/Dashboard.vue`
2.  Create a new route for `/dashboard`
3.  Update our navigation on `src/App.vue`

**src/views/Dashboard.vue**

    <template>
      <div>
        <h1>Dashboard</h1>
      </div>
    </template>
    

    <script>
    export default {
      name: 'DashboardPage'
    }
    </script>
    

**src/router.js**

    import Vue from 'vue'
    import Router from 'vue-router'
    import Home from './views/Home.vue'
    
    Vue.use(Router)
    
    export default new Router({
      mode: 'history',
      base: process.env.BASE_URL,
      routes: [
        {
          path: '/',
          name: 'home',
          component: Home
        },
        {
          path: '/about',
          name: 'about',
          // route level code-splitting
          // this generates a separate chunk (about.[hash].js) for this route
          // which is lazy-loaded when the route is visited.
          component: () => import(/* webpackChunkName: "about" */ './views/About.vue')
        },
        {
          path: '/login',
          name: 'login',
          component: () => import('./views/Login.vue')
        },
        {
          path: '/dashboard',
          name: 'dashboard',
          component: () => import('./views/Dashboard.vue')
        }
      ]
    })
    

**src/App.vue**

    <template>
      <v-app>
        <v-app-bar app color="primary" dark>
          <v-toolbar-title>Vuetify Dashboard</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn 
            v-for="link in links"
            :key="`${link.label}-header-link`"
            text 
            rounded 
            :to="link.url"
          >
            {{ link.label }}
          </v-btn>
        </v-app-bar>
        <v-content>
          <router-view></router-view>
        </v-content>
        <v-footer color="primary lighten-1" padless>
          <v-layout justify-center wrap>
            <v-btn
              v-for="link in links"
              :key="`${link.label}-footer-link`"
              color="white"
              text
              rounded
              class="my-2"
              :to="link.url"
            >
              {{ link.label }}
            </v-btn>
            <v-flex primary lighten-2 py-4 text-center white--text xs12>
              {{ new Date().getFullYear() }} — <strong>Vuetify Dashboard</strong>
            </v-flex>
          </v-layout>
        </v-footer>
      </v-app>
    </template>
    

    <script>
    export default {
      name: 'App',
      data () {
        return {
          links: [
            {
              label: 'Home',
              url: '/'
            },
            {
              label: 'Login',
              url: '/login'
            },
            {
              label: 'Dashboard',
              url: '/dashboard'
            }
          ]
        }
      }
    }
    </script>
    

Display a Table of Data on the Dashboard
----------------------------------------

* * *

Now that we have our page up and running. We need to fulfill the following requirements:

1.  User needs to display a table of data
2.  User can sort the data in the table
3.  User can determine how much data to display
4.  User can paginate through data list
5.  User should be notified when they click on a row in the table

If we had to build this from scratch, this would be quite a bit of work! Since we’re working with Vuetify though, let’s see if they have what we need by visiting [the official docs](https://vuetifyjs.com/en/getting-started/quick-start).

When we scroll through the UI components section, we’ll find a Tables section. When we expand the section, you’ll see that there are a few types of table components in here. After checkingn each out, it looks like [Data Tables](https://vuetifyjs.com/en/components/data-tables) covers most of the requirements.

It is able to:

*   Display data
*   Sort data by column
*   Change the number of rows displayed
*   Paginate through data

And since the Usage demo looks good for our needs, let’s copy it into our app.

When we open up the Source Code tab, you’ll notice that this Usage demo has two blocks: (1) Template and (2) Script. We’ll need to copy both of these into our app in order to get a functioning prototype.

![Screenshot of data table source code in docs](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707027983_06-data-table-source-code-template.png?alt=media&token=85573385-2bf0-42b9-819e-33bd9d8946bc)

**src/views/Dashboard.vue**

    <template>
      <div>
        <h1>Dashboard</h1>
        <v-data-table
          :headers="headers"
          :items="desserts"
          :items-per-page="5"
          class="elevation-1"
        ></v-data-table>
      </div>
    </template>
    

    <script>
      export default {
        name: 'DashboardPage',
        data () {
          return {
            headers: [
              {
                text: 'Dessert (100g serving)',
                align: 'left',
                sortable: false,
                value: 'name',
              },
              { text: 'Calories', value: 'calories' },
              { text: 'Fat (g)', value: 'fat' },
              { text: 'Carbs (g)', value: 'carbs' },
              { text: 'Protein (g)', value: 'protein' },
              { text: 'Iron (%)', value: 'iron' },
            ],
            desserts: [
              {
                name: 'Frozen Yogurt',
                calories: 159,
                fat: 6.0,
                carbs: 24,
                protein: 4.0,
                iron: '1%',
              },
              {
                name: 'Ice cream sandwich',
                calories: 237,
                fat: 9.0,
                carbs: 37,
                protein: 4.3,
                iron: '1%',
              },
              // The rest of the data was removed 
              // to shorten the length of the code sample 
              // in this article
            ],
          }
        },
      }
    </script>
    

Looks great! Now that we have our data table in our page. Let’s talk a closer look at how everything works.

### Headers Prop

* * *

This allows us to define the table headers. It takes an array that is made up of objects that can contain the following properties:

*   `text` (String) - What is displayed on the user side
*   `value` (String) - The value assigned to v-model for data tracking
*   `align` (String) (optional) - Allows the user to align text by ‘left’ | ‘center’ | ‘right’. (Text is aligned left by default)
*   `sortable` (Boolean) (optional) - Allows the user to sort the column. (It is `true` by default)

For more information, search for “headers” in the API section of the [Data Table docs](https://vuetifyjs.com/en/components/data-tables).

### Items Prop

* * *

The item prop takes an array of objects that will determine the data that will be displayed in the table. The key-value pair should match the headers to ensure that everything is displayed properly.

Notify the Users of Interaction
-------------------------------

* * *

Before we wrap up this lesson, we have one more requirement to fill:

1.  User should be notified when they click on a row in the table

There are many ways to approach this, but for this lesson, I’m going to introduce you to the [Snackbars component](https://vuetifyjs.com/en/components/snackbars). This component allows us to trigger a small bar to appear on the bottom of the screen in response to an element.

![Screenshot of Snackbars docs](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707029518_07-snackbars-docs.png?alt=media&token=dfe307fc-3a66-4866-b7fe-5ac295d0d94c)

When we look at the Source Code tab for the Usage demo, we can see that the visibility of the snackbar is determined by this `snackbar` property, and then we need to simply define the text being displayed.

![Screenshot of Snackbar source code](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707031176_08-snackbar-data.png?alt=media&token=b04a7e41-9a30-43b4-bb18-2ee3309b713c)

Let’s go ahead and copy this over to our app!

**src/views/Dashboard.vue**

    <template>
      <div>
        <h1>Dashboard</h1>
        <v-data-table
          :headers="headers"
          :items="desserts"
          :items-per-page="5"
          class="elevation-1"
        ></v-data-table>
        <v-snackbar
          v-model="snackbar"
        >
          {{ text }}
          <v-btn
            color="pink"
            text
            @click="snackbar = false"
          >
            Close
          </v-btn>
        </v-snackbar>
      </div>
    </template>
    

    <script>
      export default {
        name: 'DashboardPage',
        data () {
          return {
            snackbar: false,
            headers: [
              {
                text: 'Dessert (100g serving)',
                align: 'left',
                sortable: false,
                value: 'name',
              },
              { text: 'Calories', value: 'calories' },
              { text: 'Fat (g)', value: 'fat' },
              { text: 'Carbs (g)', value: 'carbs' },
              { text: 'Protein (g)', value: 'protein' },
              { text: 'Iron (%)', value: 'iron' },
            ],
            desserts: [
              {
                name: 'Frozen Yogurt',
                calories: 159,
                fat: 6.0,
                carbs: 24,
                protein: 4.0,
                iron: '1%',
              },
              {
                name: 'Ice cream sandwich',
                calories: 237,
                fat: 9.0,
                carbs: 37,
                protein: 4.3,
                iron: '1%',
              },
              // The rest of the data was removed 
              // to shorten the length of the code sample 
              // in this article
            ],
          }
        },
      }
    </script>
    

In order to trigger the snackbar appearing though, we will need an event that fires when the user clicks on the table. Let’s dive back into the [Data Tables docs](https://vuetifyjs.com/en/components/data-tables) to see if we can find what we need.

In the API section, we have been primarily focused on the API section, but as you can see here, there are other sections that are available to us as well (i.e., Slots and Events). For our current purposes, the Events sounds like what we need!

![Screenshot of Events API for Data Table](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707032298_09-data-table-events-docs.png?alt=media&token=fed94d28-2bef-4278-a68e-f0ed3b76429f)

And immediately we’re greeted by the event that we need, which is `click:row`. Let’s go ahead and add it to our code!

**src/views/Dashboard.vue**

    <template>
      <div>
        <h1>Dashboard</h1>
        <v-data-table
          :headers="headers"
          :items="desserts"
          :items-per-page="5"
          class="elevation-1"
          @click:row="selectRow"
        ></v-data-table>
        <v-snackbar
          v-model="snackbar"
        >
          {{ text }}
          <v-btn
            color="pink"
            text
            @click="snackbar = false"
          >
            Close
          </v-btn>
        </v-snackbar>
      </div>
    </template>
    

    <script>
      export default {
        name: 'DashboardPage',
        data () {
          return {
            snackbar: false,
            headers: [
              // Data hidden for brevity
            ],
            desserts: [
              // Data hidden for brevity
            ],
          }
        },
        methods: {
          selectRow() {
            this.snackbar = true
          }
        }
      }
    </script>
    

Great! Now we can open a modal, we need to be to inform users what they selected. As with any event in JavaScript, we are passed the event details. And since Vuetify automatically includes the item for the row with the event, configuring the snackbar is as simple as:

**src/views/Dashboard.vue**

    <template>
      <div>
        <h1>Dashboard</h1>
        <v-data-table
          :headers="headers"
          :items="desserts"
          :items-per-page="5"
          class="elevation-1"
          @click:row="selectRow"
        ></v-data-table>
        <v-snackbar
          v-model="snackbar"
        >
          You have selected {{ currentItem }}
          <v-btn
            color="pink"
            text
            @click="snackbar = false"
          >
            Close
          </v-btn>
        </v-snackbar>
      </div>
    </template>
    

    <script>
      export default {
        name: 'DashboardPage',
        data () {
          return {
            currentItem: '',
            snackbar: false,
            headers: [
              // Data hidden for brevity
            ],
            desserts: [
              // Data hidden for brevity
            ]
          }
        },
        methods: {
          selectRow(event) {
            this.snackbar = true
            this.currentItem = event.name
          }
        }
      }
    </script>
    

And with that, we’ve fulfilled all of our requirements. Hooray! 🎉

![Screenshot of final product for Lesson 3](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1565707033813_10-final-with-snackbar.png?alt=media&token=348c9a0b-4db2-4ea3-a97a-2289513d83b6)

Let’s ReVue
-----------

* * *

In this lesson, we have:

*   Configured routing in our app
*   Dove deeper into more advanced aspects of documentation
*   How to use more complex components

For our next lesson, we are going to talk about a critical aspect to making any application more visual depth and allow for a better user experience: layout!

Coding Challenge
----------------

* * *

For this lesson’s coding challenge:

1.  Download the repo for this course
2.  Use `src/data/employees.json` as the data source for the data table component
3.  Add multi-column sorting on the data table
4.  Update Snackbar to display Employee Name and Title

Good luck!

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-3-BEGIN)
    
*   [Finished Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-3-FINISH)
