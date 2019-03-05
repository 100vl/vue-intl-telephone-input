# vue-intl-telephone-input
Vue component for input international telephone numbers and validating.

[Demo](http://zijie-li.github.io/vue-intl-telephone-input/)

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

- In your page component:
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
          * @param {string} countryCode
          */
          onSuccess({ number, countryCode }) {
            console.log(number, countryCode);
          }
       },
     }
     </script>
     ```

- Import in single component:
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
  | `options.className` | `String` | `''` | Set custom css class name |
  | `options.input` | `Object` | | Set input attribute |
  | `options.input.required` | `Boolean` | `false` | Required property for HTML5 required attribute |
  | `options.input.readonly` | `Boolean` | `false` | Set readonly attribute |
  | `options.input.placeholder` | `String` | `''` | Set placeholder attribute |



  | 參數 | 型態 | 預設值 | 描述 |
  | -------------- | ---- | ------------- | ----------- |
  | `countryCode` | `String` | `'tw'` | 預設國家代碼 ex: 'tw', 'jp', 'kr' |
  | `dialCode` | `String` | `''` | 國際碼 ex: '886', '81', '82' |
  | `value` | `String` | `''` | 電話號碼 |
  | `options` | `Object` | `{}` | 客制選項 |
  | `options.className` | `String` | `''` | 設置客制 css class 名稱 |
  | `options.input` | `Object` | | 設定 input 的屬性 |
  | `options.input.required` | `Boolean` | `false` | 是否為必填 |
  | `options.input.readonly` | `Boolean` | `false` | 是否為不能更改 |
  | `options.input.placeholder` | `String` | `''` | 設定預設顯示字串 |

### Events

  | Property value | Arguments | Description |
  | -------------- | --------- | ----------- |
  | `validateSuccess` | `Object` | Fires when the input changes on validate success with the argument is the object includes `{ number, countryCode }` |
  | `validateError` | | Fires when the input changes on validate error |



  | 事件名稱 | 參數型別 | 描述 |
  | -------------- | --------- | ----------- |
  | `validateSuccess` | `Object` | 電話格式正確時觸發，會回傳 `{ number, countryCode }` |
  | `validateError` | | 電話格式錯誤時觸發，不會回傳任何參數 |


### Override Style

- you can set `options.className` :

  ```js
  <template>
  ...
    <intl-tel-input :options="otipns"></intl-tel-input>
  ...
  <template>
  <script>
    export default {
      data() {
        return {
          otipns: {
            className: 'my-style'
          }
        };
      }
    }
  </script>
  <style>
    .intl-tel-input.my-style .conutry-list .category-box .category-box-header {
      background-color: #3f51b5;
    }
  </style>
  ```


