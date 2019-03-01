# vue-intl-telephone-input
Vue component for input international telephone numbers and validating.

Demo [Demo pages](http://zijie-li.github.io/vue-intl-telephone-input/).

<p>
<img width="400px" src="https://i.imgur.com/8thaAbv.png"/>
</p>

## Installation
- **yarn**:
  ```bash
    yarn add vue-intl-telephone-input
  ```
- **npm**:
  ```bash
    npm i --save vue-intl-telephone-input
  ```

## Usage
- Install as a global component:
    ```javascript
    import IntlTelInput from 'vue-intl-telephone-input';

    Vue.component('intl-tel-input', IntlTelInput);
    ```

- In your component:
     ```js
     <template>
     ...
        <intl-tel-input :countryCode="'tw'" @validateSuccess="onSuccess"></intl-tel-input>
     ...
     <template>
     <script>
     export default {
       data() {
         return {
           phone: '',
         };
       },
       methods: {
         /**
          * @param {string} number
          * @param {string} country
          */
          onSuccess({ number, countryCode }) {
            console.log(number, countryCode);
          }
       },
     }
     </script>
     ```

- Add a component:
  ```js
  <template>
    <intl-tel-input :countryCode="'tw'"></intl-tel-input>
  </template>

  <script>
  import IntlTelInput from 'vue-intl-telephone-input';

  export default {
    name: 'Home',
    components: {
      IntlTelInput,
    }
  };
  </script>
  ```

### Props

  | Property value | Type | Default value | Description |
  | -------------- | ---- | ------------- | ----------- |
  | `countryCode` | `String` | `'tw'` | Default country. ex: 'tw', 'jp', 'kr' |
  | `dialCode` | `String` | `''` | Enter a country dial code. ex: '886', '81', '82' |
  | `value` | `String` | `''` | Enter a phone number |
  | `options` | `Object` | `{}` | Custom Options |
  | `options.separateDialCode` | `Boolean` | `false` | Set use separate |
  | `options.customCss` | `String` | `''` | Set custom css name |
  | `options.input` | `Object` | | Set input attribute |
  | `options.input.required` | `Boolean` | `Boolean` | Required property for HTML5 required attribute |
  | `options.input.readonly` | `Boolean` | `Boolean` | Set readonly attribute |
  | `options.input.placeholder` | `String` | `''` | Set placeholder attribute |


### Events

  | Property value | Arguments | Description |
  | -------------- | --------- | ----------- |
  | `validateSuccess` | `Object` | Fires when the input changes on validate success with the argument is the object includes `{ number, countryCode }` |
  | `validateError` | | Fires when the input changes on validate error |
