Forms: Fundamentals
===================

Introduction
------------

* * *

Forms are one of the most integral parts to any web applications since they allow the user to provide input back to an application.

In this lesson, we will be building a Signup page for our app while learning about:

*   How to create forms in Vuetify
*   Common input types you should know about
*   What pickers are and how we can use them to enhance the user experience of our forms

Let’s jump right in!

Creating a form with Vuetify
----------------------------

* * *

When you need to create a form with Vuetify, just like you would in standard HTML, you start by creating a root `v-form` element.

    <!-- Standard HTML -->
    <form>
      ...
    </form>
    
    <!-- Vuetify -->
    <v-form>
      ...
    </v-form>
    

Of course, the real question that you may be wondering is: What about the form inputs?

Well, in standard HTML, a normal text input would look like this:

    <!-- Standard HTML input -->
    <label for="name">Name</label>
    <input type="text" id="name" />
    

Even with a simple text field input, this is comprised of quite a few parts parts:

*   Separate elements
*   Label name
*   Input type
*   Input `id` attribute to pair with label `for` attribute

With Vuetify though, a text input can be condensed into a single line of code.

    <!-- Vuetify text input -->
    <v-text-field label="Name" type="text" />
    

![Diagram of how text field attributes map to standard HTML input and label elements](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910124934_01-text-field-arrows.jpeg?alt=media&token=b1d7217c-f05e-4e43-b809-36fde4018d89)

This allows you to focus on what is important (i.e., defining proper labels and types). And we no longer have to ensure that the `label` and `input` elements are paired via the correct attributes since Vuetify takes care of that for us.

And thus, with a single line of code, we get a beautiful text input with the proper labels.

![Demo of what the Vuetify text field looks like](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910126813_02-text-field.gif?alt=media&token=748a88f7-6cd0-4682-9d4d-5e1b8b5ce373)

With that, it’s time to build our Signup page!

Building Our Signup Page
------------------------

* * *

We will start by adding a Signup route to `router.js`.

**src/router.js**

    Vue.use(Router)
    
    export default new Router({
      ...
      routes: [
        ...
        {
          path: '/signup',
          name: 'signup',
          component: () => import('./views/Signup.vue')
        }
      ]
    })
    

Next, we need to create our `Signup.vue` file with a little grid layout help that we learned about earlier this course!

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup Page</h1>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

![Screenshot of Signup page scaffolded](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910127556_03-scaffolded-signup.png?alt=media&token=a9461e52-a2eb-4f2b-b448-0394462c06f6)

After that, it’s time for us to create our Signup form!

Signup Form Requirements
------------------------

* * *

For our Signup form, we have the following requirements:

A user should be able to:

*   Enter their email address
*   Select which browser they use
*   Attach their profile photo
*   Input their birthday
*   Agree to terms and conditions

### A user should be able to enter their email address

* * *

To get started, all we need to do is add our root element `v-form` with a `v-text-field` component with a type of `email` and a label of `Email`.

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup Page</h1>
            <v-form>
              <v-text-field label="Email" type="email" />
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

![Recording of email input on Signup page](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910128534_04-email-input.gif?alt=media&token=c010be7f-e91e-4f4e-ae55-891022880dfc)

### A user should be able to select which browser they use

* * *

When we look at the documentation for form inputs in Vuetify, one might be tempted to use the standard [select menu](https://vuetifyjs.com/en/components/selects). However, I would like to introduce you to a more complex component that many developers can attest is quite difficult to build from scratch: [autocomplete](https://vuetifyjs.com/en/components/autocompletes).

The autocomplete component is fantastic enhancement on the standard select menu since it allows users to type their selection and use partial match to filter down results. This makes for a phenomenal user experience when there are many options to choose from. More importantly, if users prefer the standard scrolling and clicking experience of a select menu, it still works as desired.

And while the code for an autocomplete can be fairly complex, using Vuetify’s autocomplete component is pretty much as simple as the text-field input. All you need to do is provide it:

*   Label
*   Array of items that you want to populate it with

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup Page</h1>
            <v-form>
              <v-text-field label="Email" type="email" />
              <v-autocomplete label="Which browser do you use?" :items="browsers" />
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        browsers: ['Chrome', 'Firefox', 'Safari', 'Edge', 'Brave']
      })
    }
    </script>
    

![Recording of autocomplete select in action](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910129331_05-select-browser.gif?alt=media&token=e56b58c9-0714-4ef8-b6d9-c69493ca69ea)

And that’s it! It works like magic! 💫

### A user should be able to attach their profile photo

* * *

When it comes to attaching files, those familiar with HTML might be inclined to use `<v-text-field type="file" />`. However, Vuetify makes your life even easier by including the [file input component](https://vuetifyjs.com/en/components/file-inputs) which provides a well designed input that makes it obvious to users they are about to attach a file.

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup Page</h1>
            <v-form>
              <v-text-field label="Email" type="email" />
              <v-autocomplete label="Which browser do you use?" :items="browsers" />
              <v-file-input label="Attach profile picture" />
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        browsers: ['Chrome', 'Firefox', 'Safari', 'Edge', 'Brave']
      })
    }
    </script>
    

![Screenshot of what the file input component looks like](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910130632_06-file-input.png?alt=media&token=8967f77f-d229-410b-b2ac-458359408bee)

### A user should be able to input their birthday

* * *

When it comes to date inputs, this is one of the hardest form inputs that most developers encounter in their careers. While the built-in browser date picker is already quite powerful, stakeholders often want something with a sleek design. As a result, it’s time to introduce you to Vuetify Pickers, which are a series of components that enhance the user experience of your forms by providing your users with rich user interfaces for things such as [choosing color](https://vuetifyjs.com/en/components/color-pickers), [choosing a time](https://vuetifyjs.com/en/components/time-pickers), or [picking a date](https://vuetifyjs.com/en/components/date-pickers).

As you might suspect, using Vuetify’s datepicker is no harder than a single line of code!

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup Page</h1>
            <v-form>
              <v-text-field label="Email" type="email" />
              <v-autocomplete label="Which browser do you use?" :items="browsers" />
              <v-file-input label="Attach profile picture" />
              <v-date-picker />
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        browsers: ['Chrome', 'Firefox', 'Safari', 'Edge', 'Brave']
      })
    }
    </script>
    

![Screenshot of the date picker component on the Signup page](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910131253_07-datepicker.png?alt=media&token=35bc933f-9a5f-424c-8a80-809adfcaea97)

While the date picker looks great in our form, this doesn’t inform users that the date picker should be used for their birthday. So what we need to do is:

1.  Add a text-field component with a label of birthday
2.  Attach the same `v-model` data property to the text-field and date-picker component
3.  Add a `readonly` property to the text-field so users are encouraged to use the date-picker

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup Page</h1>
            <v-form>
              <v-text-field label="Email" type="email" />
              <v-autocomplete label="Which browser do you use?" :items="browsers" />
              <v-file-input label="Attach profile picture" />
              <v-text-field label="Birthday" v-model="birthday" readonly />
              <v-date-picker v-model="birthday" />
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        birthday: '',
        browsers: ['Chrome', 'Firefox', 'Safari', 'Edge', 'Brave']
      })
    }
    </script>
    

![Screenshot of date picker component with text field paired with it](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910132629_08-datepicker-with-text.png?alt=media&token=154ba491-224a-470c-9a4b-7da762984110)

Now, you may be wondering how we should toggle the visibility of the date picker based on whether the user is focused on the text input. You would be correct in this instinct since the date picker takes up a lot of room on the page.

In order to achieve this in a way that ensures the form inputs are accessible to all users, the required code goes beyond the scope of this course since it requires advanced knowledge of Vue. However, you can find an example of how this should be done in the [Examples section of the date picker docs](https://vuetifyjs.com/en/components/date-pickers#examples) under “Date pickers - In dialog and menu.”

### A user should be able to agree to terms and conditions

* * *

Finally, to wrap up our form, all we need is a checkbox for users to agree to the terms and conditions. And while you may think this is accomplished using the `type="checkbox"` HTML prop, Vuetify makes it even easier for you with the [checkbox component](https://vuetifyjs.com/en/components/selection-controls)!

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup Page</h1>
            <v-form>
              <v-text-field label="Email" type="email" />
              <v-autocomplete label="Which browser do you use?" :items="browsers" />
              <v-file-input label="Attach profile picture" />
              <v-text-field label="Birthday" v-model="birthday" readonly />
              <v-date-picker v-model="birthday" />
              <v-checkbox label="Agree to terms & conditions" />
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        birthday: '',
        browsers: ['Chrome', 'Firefox', 'Safari', 'Edge', 'Brave']
      })
    }
    </script>
    

![Screenshot of checkbox component in Signup page](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910133856_09-checkbox.png?alt=media&token=37b2c331-04d6-46c0-8aad-be594823b4bf)

And for those who love the switch design pattern:

![Demo of switch component design pattern](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910134889_10-switch.gif?alt=media&token=669b958e-6f98-4c3c-9726-d2797b6a7234)

You could easily swap out the `v-checkbox` component for `v-switch`! However, since the checkbox is more standard for this type of question, we will leave it as is.

### Final touches

* * *

Now that we have completed our user requirements, we just need to add a submit button to our form. After all, how else will our users submit their request otherwise?

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup Page</h1>
            <v-form>
              <v-text-field label="Email" type="email" />
              <v-autocomplete label="Which browser do you use?" :items="browsers" />
              <v-file-input label="Attach profile picture" />
              <v-text-field label="Birthday" v-model="birthday" readonly />
              <v-date-picker v-model="birthday" />
              <v-checkbox label="Agree to terms & conditions" />
              <v-btn type="submit" color="primary">Submit</v-btn>
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        birthday: '',
        browsers: ['Chrome', 'Firefox', 'Safari', 'Edge', 'Brave']
      })
    }
    </script>
    

![Screenshot of completed form](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1569910135787_11-final-form.png?alt=media&token=acc81d82-7360-44cc-847a-ede5929a14ee)

And with that, our Signup form is complete! 🎉

Let’s ReVue
-----------

* * *

In this lesson, we learned about:

*   The basics for creating forms with Vuetify
*   Common input types that you should know about
*   What picker components are and how to use them to make your form experience awesome for your users

While we have only used a few of the form input types that Vuetify offers us, I hope you caught a glimpse of how easy forms can be when you use Vuetify.

In the next lesson, we will be covering another important aspect of forms: validations. See you there!

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-6-BEGIN)
    
*   [Finished Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-6-FINISH)
