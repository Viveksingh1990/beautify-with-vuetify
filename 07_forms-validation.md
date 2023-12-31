Forms: Validation
=================

Form Validation
---------------

* * *

One of the key features of a good form is the ability to validate your user’s input. After all, this helps to ensure that they are providing the required information in the correct format. And while all form inputs should use HTML attributes as a starting point for validation, there typically comes a point where you need custom rules along with custom validation methods. This is where Vuetify is here to help.

Rules
-----

* * *

By wrapping our form in a `v-form` component, this enables us to use the `rules` prop on any Vuetify input component. The `rules` prop takes an array of functions that allow us to define when the value being passed in is valid or not.

All form input components in Vuetify have a `rules` prop that takes an array of functions that allow you to specify the conditions for whether the value is valid or not. Each function inside of the array is its own individual rule. As a result, if any of the functions inside the array return `false` or a String, which is typically the error message you want to display to the user, then the input is considered invalid.

In our Vuetify Dashboard’s Signup page, we have some form fields that are required in order for the user to create their account:

1.  Agree to terms and conditions
2.  Email

### Creating validation for agree to terms and conditions

* * *

When creating a rule in the `rules` prop array, each rule is an anonymous function that provides the value of the input.

    value => {
      // Check if the value has at least 1 character
      if (value.length > 0) {
        // Since this is true, we can return true
        return true
      } else {
        // If it is false, then display the returned String
        return 'Error message to display to user'
      }
    }
    

However, it is typically written in a shorthand version:

    value => value > 0 || 'Error message to display to user'
    

Since the “Agree to terms and conditions” is required, we need to attach a `v-model` to our checkbox and add a `rules` prop that checks whether it is true or not.

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup</h1>
            <v-form>
              <!-- Other form elements hidden for brevity -->
              ...
              <v-checkbox
                label="Agree to terms & conditions"
                v-model="agreeToTerms"
                :rules="agreeToTermsRules"
                required
              ></v-checkbox>
              <v-btn type="submit" color="primary">Submit</v-btn>
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        agreeToTerms: false,
        agreeToTermsRules: [
          value =>
            !!value ||
            'You must agree to the terms and conditions to sign up for an account.'
        ],
        // Other data hidden for brevity
        ...
      })
    }
    </script>
    

And with that, we have error validation on the “Agree to terms and conditions” checkbox!

### Creating validation for our email input

* * *

For our email input validation, it is a little bit more complicated. While there are many ways to validate for proper email inputs, we will use the following criteria:

*   Email is required
*   Email should have a username of at least 1 character
*   Email should contain an @ symbol
*   Email should contain a domain name with at least one character, a period, and at least two letter extension

When creating an array of rules for an input, each rule will be show to the user in order. As a result, it’s important to consider the proper order that users should see error messages.

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup</h1>
            <v-form>
              <v-text-field
                label="Email"
                type="email"
                v-model="email"
                :rules="emailRules"
              ></v-text-field>
              <!-- Other form elements hidden for brevity -->
              ...
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        // Other data hidden for brevity...
        email: '',
        emailRules: [
          value => value.indexOf('@') !== 0 || 'Email should have a username.',
          value => value.includes('@') || 'Email should include an @ symbol.',
          value =>
            value.indexOf('.') - value.indexOf('@') > 1 ||
            'Email should contain a valid domain.',
          value => value.includes('.') || 'Email should include a period symbol.',
          value =>
            value.indexOf('.') <= value.length - 3 ||
            'Email should contain a valid domain extension.'
        ]
      })
    }
    </script>
    

How to Reset Forms
------------------

* * *

There are two types of resets when it comes to forms:

*   `reset()` - Resetting the entirety of the form (including the form input values)
*   `resetValidation()` - Resetting only the validation of the form

However, we need to be able to refer to the form that we want to trigger it on. In order to do this, we assign our `v-form` element with a `ref` prop that is specific to Vue and allows you to reference a specific HTML element. So in our form example, let’s give our form a `ref` of `loginForm`.

Once we assign the `ref`, it can now be programmatically referred to using the `$refs` object, which contains any `ref`s that have been defined. As a result, we can now create two buttons:

*   One to reset the validation on our form
*   Another to reset the entirety of our form

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup</h1>
            <v-form ref="signUpForm">
              <!-- Form input elements hidden for brevity -->
              ...
              <v-btn type="submit" color="primary" class="mr-4">
                Submit
              </v-btn>
              <v-btn color="warning" class="mr-4" @click="resetValidation">
                Reset Validation
              </v-btn>
              <v-btn color="error" @click="resetForm">Reset</v-btn>
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        // Data store hidden for brevity
        ...
      }),
      methods: {
        resetForm() {
          this.$refs.signUpForm.reset()
        },
        resetValidation() {
          this.$refs.signUpForm.resetValidation()
        }
      }
    }
    </script>
    

Validate
--------

* * *

When it comes to checking whether or not our form is valid, this is typically done at the time of submission. However, there will be times when you need to programmatically check whether or not a form is valid in order to perform other actions (i.e., trigger notifications, disable buttons, etc.). Vuetify makes this easy for us by providing us two ways for checking:

*   Every `v-form` has a `value` prop which tracks whether or not it is valid
*   And it has a `validate` function to trigger a manual check on whether the form is valid or not.

For our Signup form, let’s go ahead and disable our Submit button until the form is valid.

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup</h1>
            <v-form ref="signUpForm" v-model="formValidity">
              <!-- Other form input elements hidden for brevity -->
              ...
              <v-btn
                type="submit"
                color="primary"
                class="mr-4"
                :disabled="!formValidity"
              >
                Submit
              </v-btn>
              ...
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        // Other data hidden for brevity
        ...
        formValidity: false
      }),
      methods: {
        // Methods hidden for brevity
        ...
      }
    }
    </script>
    

Finally, let’s add a button for the user to manully validate the form. Just like the `reset` and `resetValidation` function, it can be triggered by referencing the form via its `ref` property.

**src/views/Signup.vue**

    <template>
      <v-container>
        <v-row>
          <v-col>
            <h1>Signup</h1>
            <v-form ref="signUpForm" v-model="formValidity">
              <!-- Other form elements hidden for brevity -->
              ...
              <v-btn color="success" class="mr-4" @click="validateForm">
                Validate Form
              </v-btn>
              <v-btn color="warning" class="mr-4" @click="resetValidation">
                Reset Validation
              </v-btn>
              <v-btn color="error" @click="resetForm">Reset</v-btn>
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </template>
    

    <script>
    export default {
      data: () => ({
        // Data store hidden for brevity
        ...
      }),
      methods: {
        resetForm() {
          this.$refs.signUpForm.reset()
        },
        resetValidation() {
          this.$refs.signUpForm.resetValidation()
        },
        validateForm() {
          this.$refs.signUpForm.validate()
        }
      }
    }
    </script>
    

And with that, our Signup form is complete with form validation!

Final Thoughts
--------------

* * *

Congratulations, you now possess all of the fundamental knowledge you need to succesfully build forms with custom validations.

For those who want to use other validation libraries like Vuelidate and Vee-validate, you can find examples for how to do so in the:

*   [Vee-validate sections of the docs](https://vuetifyjs.com/en/components/forms#vee-validate)
*   [Vuelidate section of the docs](https://vuetifyjs.com/en/components/forms#vuelidate)

See you in the next lesson!

### Lesson Resources

##### Source Code:

*   [Starting Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-7-BEGIN)
    
*   [Finished Code](https://github.com/Code-Pop/beautify-with-vuetify/tree/Lesson-7-FINISH)
