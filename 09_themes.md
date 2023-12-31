Themes
======

Introduction
------------

While component libraries can make our lives much easier, one common downside is that every website you build will look the same. As a result, it’s critical for us to be able to customize the theme so we can utilize the power of component libraries while creating unique designs.

In this lesson, we will be learning about:

*   Basics behind Vuetify themes
*   How to toggle between light and dark themes
*   And how to customize theme variables

The Basic of Themes
-------------------

For most websites, they often display using a light theme:

However, as many developers know, being able to choose your theme can greatly impact the readability and overall experience. For example, many developers prefer writing code in the dark theme.

When working with themes in Vuetify, as you might expect, there is no better place to get started than the [Vuetify theme docs](https://vuetifyjs.com/en/customization/theme). And for those who are interested in paid themes, be sure to check out their [Premium themes marketplace](https://vuetifyjs.com/en/themes/premium)!

Toggling Light and Dark Themes
------------------------------

In our Vuetify Dashboard app, let’s start by configuring our Vuetify theme to use the dark theme. To do this, we configure out Vuetify configuration object with the `theme` property and pass it a key value pair of: `dark: true`.

**vuetify.js**

    import Vue from 'vue'
    import Vuetify from 'vuetify/lib'
    
    Vue.use(Vuetify)
    
    export default new Vuetify({
      icons: {
        iconfont: 'mdi'
      },
      theme: {
        dark: true
      }
    })
    

While this is a fairly straightforward change, it would be more helpful to our users if we gave them the ability to choose which theme they preferred. As a result, let’s go ahead and add a button in the navigation bar that allows them to do that:

**App.vue**

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
          <v-btn @click="toggleTheme" text rounded>
            Toggle Theme
          </v-btn>
        </v-app-bar>
        ...
      </v-app>
    </template>
    

Now we need to create our `toggleTheme` method that will allow users to update the theme. And to do this, we refer to our global `$vuetify` object and toggle the property `theme.dark` which takes a Boolean value.

**App.vue**

    <script>
    export default {
      name: 'App',
      data() {
        return {
          ...
        }
      },
      methods: {
        toggleTheme() {
          this.$vuetify.theme.dark = !this.$vuetify.theme.dark
        }
      }
    }
    </script>
    

And with that, your users can now toggle the theme!

Applying Custom Colors to a Theme
---------------------------------

In order to apply custom color palettes to our theme, we can configure this globally by passing defining the `themes` property in the `theme` property:

**vuetify.js**

    import Vue from 'vue'
    import Vuetify from 'vuetify/lib'
    
    Vue.use(Vuetify)
    
    export default new Vuetify({
      icons: {
        iconfont: 'mdi'
      },
      theme: {
        themes: {
          light: {
            primary: '#41B883'
          },
          dark: {
            primary: '#34495E'
          }
        }
      }
    })
    

For a list of all the various color properties in a theme, you can find them in the [Theme: Customizing](https://vuetifyjs.com/en/customization/theme#customizing) section of the docs.

However, one thing you may have noticed is that our links in the dark theme are hard to read. One way we can fix this is to define the colors for `anchor` in our theme configuration.

**vuetify.js**

    import Vue from 'vue'
    import Vuetify from 'vuetify/lib'
    
    Vue.use(Vuetify)
    
    export default new Vuetify({
      icons: {
        iconfont: 'mdi'
      },
      theme: {
        themes: {
          light: {
            primary: '#41B883'
          },
          dark: {
            primary: '#34495E',
            anchor: '#fff'
          }
        }
      }
    })
    

However, for the purposes of demonstrating how you can programmatically and dynamically set the colors of themes. Let’s do this inside of our `toggleTheme` method.

**App.vue**

    <script>
    export default {
      name: 'App',
      data() {
        return {
          ...
        }
      },
      methods: {
        toggleTheme() {
          this.$vuetify.theme.themes.dark.anchor = '#41B883'
          this.$vuetify.theme.dark = !this.$vuetify.theme.dark
        }
      }
    }
    </script>
    

Just like before, we can refer to the global `$vuetify` object in order to dynamically change the value of the theme color. And with just one line of code, our links look great in the dark theme now!

Defining Custom Typography
--------------------------

While the Roboto font family looks great, it is common for designers to want custom typography for their website. And while it is not as easy as defining properties inside the Vuetify configuration object, we can still accomplish it with a couple of configurations.

To start, let’s swap out the Google Font import for Roboto in `index.html` for another font pair: Fira Sans and Merriweather.

**public/index.html**

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width,initial-scale=1.0" />
        <link rel="icon" href="<%= BASE_URL %>favicon.ico" />
        <title>vuetify-dashboard</title>
        <link href="https://fonts.googleapis.com/css?family=Fira+Sans:700|Merriweather&display=swap" rel="stylesheet">
        <link
          rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/@mdi/font@latest/css/materialdesignicons.min.css"
        />
      </head>
      <body>
        <noscript>
          <strong
            >We're sorry but vuetify-dashboard doesn't work properly without
            JavaScript enabled. Please enable it to continue.</strong
          >
        </noscript>
        <div id="app"></div>
        <!-- built files will be auto injected -->
      </body>
    </html>
    

Now that we have our our new font families imported, we need to access Vuetify’s Sass variables. To do this, we start by creating our own `scss/variables.scss` file in our `src` directory.

**src**/**scss/variables.scss**

    $body-font-family: 'Fira Sans';
    $heading-font-family: 'Merriweather';
    

Once we restart our local server, Vuetify will rebuild the styles according to these new variables. And for those wondering where the variable names are coming from, they were derived from the `[variables.scss` file in the Vuetify source code\]([https://github.com/vuetifyjs/vuetify/blob/master/packages/vuetify/src/styles/settings/\_variables.scss](https://github.com/vuetifyjs/vuetify/blob/master/packages/vuetify/src/styles/settings/_variables.scss)).

For more information, you can learn more about [how Sass variables work on their docs](https://vuetifyjs.com/en/customization/sass-variables).

Conclusion
----------

And with that, you now know the basics of configuring your own theme with Vuetify. You are now ready to leverage the power of Vuetify’s component library while creating unique designs!

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-9-BEGIN)
    
*   [Finished Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-9-END)
