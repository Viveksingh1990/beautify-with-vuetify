Getting Started with Vuetify
============================

Introduction
------------

In today’s world of beautifully designed software, having a site that only works functionally without any design is no longer acceptable. Most users these days expect a certain level polish when they visit a site. So when you are starting a new website, in addition to making sure everything works properly, how do you make sure that it has:

*   an attractive visual design
*   the best user experience possible

The answer is Design Systems, which are a set of guidelines established to provide consistent design and user experience.

Some of the popular ones you may have heard of include:

*   Bootstrap
*   Bulma
*   Material Design

Material Design is a design system popularized by Google which encapsulates their design principles and guidelines.

And while getting started with Material Design may seem as simply as downloading a CSS file and including it inside your application, this method is often unable to fully take advantage of component design patterns that allow your application to sustainably scale.

Presenting Vuetify!
-------------------

Lucky for us, John Leider created Vuetify, which is a Vue component library that is built according to the Material Design specifications so you can rapidly build well designed applications. And while choosing a component library can be a difficult decision, Vuetify is a great choice because:

*   It is easy and intuitive to use
*   Has one of the most vibrant and active communities in the Vue ecosystem
*   With regularly scheduled patches, you can feel confident knowing your application will be supported and updated as fixes and new features are pushed
*   And you won’t have to worry whether the components are accessible or supported across the various browsers. Vuetify has you covered.

With that said, let’s build something with Vuetify!

For this lesson, we are going to show you how powerful Vuetify is by building a common module that most applications have: a login module.

![Login Module Preview](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1562629938952_login-module-finished.png?alt=media&token=23d7c3f5-22db-4deb-baec-6d9e662c86ce)

A login module typically consists of:

*   Text heading
*   Username input field
*   Password input field
*   Login button
*   Register button

Sounds simple enough. Let’s dive into some code!

Installing Vuetify on Your App
------------------------------

To get started, we will scaffold a new Vue app with Vue CLI 3 called `vuetify-dashboard` with the default preset.

Next, we will change into our project directory so we can add Vuetify to our app. With Vue CLI’s plugin system, adding Vuetify to our project is easy. All you have to do is run:

    vue add vuetify 
    

Then you select the `Default` preset which is great for most setups.

![Preview presets for Vuetify install](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1562629935431_install-vuetify-preset.png?alt=media&token=f438fdb3-32e2-41cd-80a4-cd5664837802)

Once the installation is complete, you will see that Vuetify has added and updated some files to make sure your app is properly configured. To make sure everything is running properly, let’s start up our local dev server. When we visit `http://localhost:8080`, you will see that Vuetify has replaced the standard boilerplate home page with its own.

![Preview Vuetify Boilerplate homepage](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1562630091514_vuetify-boilerplate-homepage.png?alt=media&token=5efae954-7b7e-4b5b-9a20-bef07a690eb7)

Exploring the Project Directory
-------------------------------

Once you open up the project in VS Code, one of the things you’ll notice in the `src/App.vue` is that there a lot of components prefixed with `v-`. Similar to how Vue uses the prefix to indicate Vue-specific directives, this is how Vuetify indicates that these components are part of its library.

This might seem like a lot to take in at first, but let’s unbox one of these components. If we take a look at what `<v-spacer>` outputs to, you’ll see that it is a simple wrapper of a `div` that has the material design CSS for `spacer` attached to it.

This is one of the simplest examples and it can get far more complicated than this (i.e., colors, size, themes, etc.), but know that these are no different than any other Vue component you would build yourself. The only difference is that they account for Material Design styles (i.e., colors, sizes and other configurations) and are intelligently designed to be composable and reusable.

Now let’s clear out the boilerplate so we can start with a fresh page.

    <template>
      <v-app>
    
      </v-app>
    </template>
    

    <script>
    export default {
      name: 'App',
      data () {
        return {
          //
        }
      }
    }
    </script>
    

Building the Login Module
-------------------------

First we will start by adding a `v-card` component to our page which will serve as our container for the login module and dropping in a `h1` to make sure everything is rendering correctly.

    <template>
      <v-app>
        <v-card>
          <h1>Login</h1>
        </v-card>
      </v-app>
    </template>
    

    <script>
    export default {
      name: 'App',
      data () {
        return {
          //
        }
      }
    }
    </script>
    

Now we will add a sub-component called `v-card-title` which provides standard spacing and positioning for the card header.

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
        </v-card>
      </v-app>
    </template>
    

Next we will add the `v-card-text` sub-component which acts as the wrapper for the body content in the `v-card`. And since we need to add some input fields, we’ll start by adding a `v-form` wrapper. Since we need text inputs, we will add a `v-text-field` component. We will then configure the labels by adding a prop of `label` with the value of “Username.”

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field label="Username" />
            </v-form>
          </v-card-text>
        </v-card>
      </v-app>
    </template>
    

When you click in the input field, the text input follows Material Design specs by changing positions of the label when focused and unfocused if no value is present.

![Recording of what input focus looks like](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1562629931457_input-animation-focus.gif?alt=media&token=04fe1b9e-6d38-4b5c-9243-09aae1632440)

Next, we will add a `v-text-field` for the password input field. As we can see here, the `v-text-field` defaults to text so we will need to define the type as password.

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field label="Username" />
              <v-text-field 
                type="Password"
                label="Password" 
              />
            </v-form>
          </v-card-text>
        </v-card>
      </v-app>
    </template>
    

And now, the input is now hidden. We still need to add our buttons. Luckily for us, `v-card` has another sub-component called `v-card-actions` which serves as the container for our buttons. Then we add our buttons, we use the `v-btn` component for Register and Login.

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field label="Username" />
              <v-text-field 
                type="Password"
                label="Password" 
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn>Register</v-btn>
            <v-btn>Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

Now that we have the main parts for our login module, we can now style to make it look even better!

Styling Our Login Module
------------------------

Let’s start by adding an icons to our text inputs. This can easily be done with the prop `prepend-icon`. As you can see here, this automatically adds a properly spaced icon before the text input. In addition, another great thing about Vuetify is that it comes with a standard icon library that is already configured and ready for use. For a list of all the available icons, check out [the docs](https://material.io/tools/icons/?style=baseline).

So for the username, we will prepend it with the icon `mdi-account-circle`. And for the password input field, let’s add prepend it with a `lock` icon

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field 
                label="Username"
                prepend-icon="mdi-account-circle"
              />
              <v-text-field 
                type="Password"
                label="Password"
                prepend-icon="mdi-lock"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn>Register</v-btn>
            <v-btn>Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

We’ll also need to add an icon at the end of the password input to inform users on whether the password is visible or not. As you might guess, Vuetify makes that easy for us with the `append-icon` prop that we will supply with the `mdi-eye-off` value.

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field 
                label="Username"
                prepend-icon="mdi-account-circle"
              />
              <v-text-field 
                type="Password"
                label="Password"
                prepend-icon="mdi-lock"
                append-icon="mdi-eye-off"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn>Register</v-btn>
            <v-btn>Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

Next, these buttons could use some color, so let’s use the “success” colors for Register and “info” colors for Login.

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field 
                label="Username"
                prepend-icon="mdi-account-circle"
              />
              <v-text-field 
                type="Password"
                label="Password"
                prepend-icon="mdi-lock"
                append-icon="mdi-eye-off"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn color="success">Register</v-btn>
            <v-btn color="info">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

Finally, let’s clean up the layout of the card. The login module is a little wide, so let’s give our card a width of `400px`, add spacing to the top and centering it Vuetify CSS utility classes `mt-5` (i.e., margin top 5 units) and `mx-a` (i.e., horizontal margin auto). The spacing between login and the username input is also a little large. Luckily Vuetify provides us with utility classes like `pb-0` which removes the bottom padding

    <template>
      <v-app>
        <v-card width="400px" class="mt-5 mx-a">
          <v-card-title class="pb-0>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field 
                label="Username"
                prepend-icon="mdi-account-circle"
              />
              <v-text-field 
                type="Password"
                label="Password"
                prepend-icon="mdi-lock"
                append-icon="mdi-eye-off"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn color="success">Register</v-btn>
            <v-btn color="info">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

Next, let’s add a `v-divider` to add visually separate the “card-actions” from the “card-text”

![Preview of login odule with divider](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1562629941489_login-with-divider.png?alt=media&token=4434f3a3-687f-4b6d-8ff9-439d9cf95617)

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field 
                label="Username"
                prepend-icon="mdi-account-circle"
              />
              <v-text-field 
                type="Password"
                label="Password"
                prepend-icon="mdi-lock"
                append-icon="mdi-eye-off"
              />
            </v-form>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="success">Register</v-btn>
            <v-btn color="info">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

And finally, to add some space between the buttons, we add `v-spacer` and then we’re good to go!

    <template>
      <v-app>
        <v-card>
          <v-card-title>
            <h1>Login</h1>
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field 
                label="Username"
                prepend-icon="mdi-account-circle"
              />
              <v-text-field 
                type="Password"
                label="Password"
                prepend-icon="mdi-lock"
                append-icon="mdi-eye-off"
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
      </v-app>
    </template>
    

![Preview of finished login module](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1562629938952_login-module-finished.png?alt=media&token=23d7c3f5-22db-4deb-baec-6d9e662c86ce)

However, as you might notice, at the moment users cannot toggle the password visibility. So let’s build that functionality next!

Toggle Password Visibility
--------------------------

To start, we are going to add a `data` property of `showPassword` that will have a default boolean value of `false`.

    <script>
    export default {
      name: 'App',
      data () {
        return {
          showPassword: false
        }
      }
    }
    </script>
    

To toggle visibility, we will use the `v-bind` directive to determine whether the type of the field will be text or password.

    <template>
      <v-app>
        <v-card width="400" class="mx-auto mt-5">
          <v-card-title class="pb-0">
            <h1>Login</h1>
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
                append-icon="mdi-eye-off"
              />
            </v-form>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="success">Register</v-btn>
            <v-btn color="info">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

Since we want to allow users to toggle password visibility by clicking on the visibility icon, we will need to add a click event to the icon. Lucky for us, Vuetify already thought of this by allowing us to target our appended-icon by passing the target `append` as an argument to the click handler. With this, we can now write the JavaScript we need to toggle the value of `showPassword`.

    <template>
      <v-app>
        <v-card width="400" class="mx-auto mt-5">
          <v-card-title class="pb-0">
            <h1>Login</h1>
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
                append-icon="mdi-eye-off"
                @click:append="showPassword = !showPassword"
              />
            </v-form>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="success">Register</v-btn>
            <v-btn color="info">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

Now when we click on our eye icon, you can see that it toggles the password visibility! However, this is still not quite right because the password is visible while the `mdi-eye-off` icon is still showing. To fix this, we use the `v-bind` directive again to write a ternary statement that will show a `mdi-eye` icon when `showPassword` is true and `mdi-eye-off` when `showPassword` is false.

    <template>
      <v-app>
        <v-card width="400" class="mx-auto mt-5">
          <v-card-title class="pb-0">
            <h1>Login</h1>
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
            <v-btn color="info">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-app>
    </template>
    

![](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1562630088856_toggle-password.gif?alt=media&token=78b69812-1ffa-4383-b697-ac2d9d560726)

Everything works and looks great now!

Conclusion
----------

So far you’ve seen a preview of how helpful Vuetify can be for creating beautiful web applications.

In the rest of the course, we’ll cover everything you need to know to start confidently using Vuetify in your own projects.

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-1-BEGIN)
    
*   [Finished Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-1-FINISH)
