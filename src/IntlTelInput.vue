<template>
  <div class="intl-tel-input" :class="currentOptions.className">
    <div class="conutry-list" :class="{ active: (!currentOptions.input.readonly & showConutryList) }">
      <div class="search-wrapper">
        <input type="text" class="search-input" v-model="searchValue" @keyup="onSearch">
      </div>
      <div class="conutry-category">
        <div
          class="category-box"
          v-for="(group, alphabet) in countriesGroups"
          :key="alphabet"
        >
          <div class="category-box-header">{{ alphabet }}</div>
          <ul class="list">
            <li class="category-box-item" v-for="country in group" :key="country.code" @click="onSelectCountry(country.code)">
              <span class="item-conutry-name">{{ country.name }}</span>
              <span class="item-conutry-code">+{{ country.dialCode }}</span>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div class="select-conutry" @click="showConutryList = !showConutryList">
      <span class="conutry-code">+{{ currentCountry.dialCode }}</span>
    </div>
    <div class="input-wrapper" :class="(this.validatorMode) ? (this.validatorStatus) ? 'success' : 'error' : ''">
      <input type="tel" class="tel-input" autocomplete="off" :placeholder="currentOptions.input.placeholder || currentCountry.phoneFormat" :required="currentOptions.input.required" :readonly="currentOptions.input.readonly" v-model="modelValue" @keyup="validatorCellphone" autofocus>
    </div>
  </div>
</template>

<script>
import countries from "./assets/countries.json";
import { parse, format, getNumberType, isValidNumber } from './assets/validate';
import "./assets/scss/intl-tel-input.css";

export default {
  name: "IntlTelInput",
  props: {
    options: {
      type: Object,
      default: () => {
        return {};
      }
    },
    countryCode: {
      type: String,
      default: 'tw'
    },
    dialCode: {
      type: String,
      default: ''
    },
    value: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      countriesGroups: countries,
      countries: Object.values(countries).flat(),
      initOptions: {
        separateDialCode: false,
        className: '',
        input: {
          required: false,
          readonly: false,
          placeholder: ''
        }
      },
      currentCountryCode: this.countryCode,
      currentDialCode: this.dialCode,
      modelValue: this.value,
      searchValue: '',
      showConutryList: false,
      validatorMode: false,
      validatorStatus: false
    };
  },
  computed: {
    currentOptions() {
      return Object.assign(this.initOptions, this.options);
    },
    currentCountry() {
      const countryCode = this.currentCountryCode.toLowerCase();
      return this.countries.find(c => c.code === countryCode);
    }
  },
  mounted() {
    // 有預設值自動檢查格式
    if (this.currentDialCode && this.value) this.validatorCellphone();
  },
  methods: {
    onSearch() {
      if (!this.searchValue) {
        this.countriesGroups = { ...countries };
        return;
      }

      // is number
      if (!isNaN(Number(this.searchValue))) {
        const initCountries = this.countries;
        let countryCopie = [];

        const newCounties = initCountries.filter((country) => {
          return (country.dialCode.toString().search(this.searchValue) !== -1);
        });

        newCounties.forEach((country) => {
          let char = country.name.substr(0, 1).toUpperCase();
          if (countryCopie[char] === undefined) countryCopie[char] = [];
          countryCopie[char].push(country);
        });

        this.countriesGroups = { ...countryCopie };
      }
      else {
        const countryCopie = { ...countries };

        let key = this.searchValue.substr(0, 1).toUpperCase();

        Object.keys(countryCopie).map((code) => {
          if (code !== key) delete countryCopie[code];
        });

        if (this.searchValue.length > 1) {
          countryCopie[key] = countryCopie[key].filter((country) => {
            return (country.name.toLowerCase().search(this.searchValue.toLowerCase()) !== -1);
          });
        }

        this.countriesGroups = countryCopie;
      }
    },
    onSelectCountry(code) {
      this.currentCountryCode = code;
      this.modelValue = '';
      this.validatorCellphone();
    },
    validatorCellphone() {
      this.validatorMode = true;
      this.validatorStatus = ((this._getNumberType() === 'MOBILE' || this._getNumberType() === 'FIXED_LINE_OR_MOBILE') && this._isValidNumber());

      if (this.validatorStatus) {
        this.$emit('validateSuccess', { number: this._getNumber(), countryCode: this.currentCountryCode });
      }
      else {
        this.$emit('validateError');
      }
    },
    _getNumber() {
      return format(this._getFullNumber(), this.currentCountryCode.toUpperCase(), 'International');
    },
    _getFullNumber() {
      let val = this.modelValue,
        dialCode = this.currentCountry.dialCode.toString(),
        prefix;
      if (this.currentOptions.separateDialCode) {
        prefix = '+' + dialCode;
      } else if (dialCode && dialCode.charAt(0) == '1' && dialCode.length == 4 && dialCode.substr(1) != val.substr(0, 3)) {
        prefix = dialCode.substr(1);
      } else {
        prefix = '';
      }
      return prefix + val;
    },
    _getNumberType() {
      try {
        let parsePhone = parse(this._getFullNumber(), this.currentCountry.code.toUpperCase());
        return getNumberType(parsePhone.phone, parsePhone.country);
      } catch (error) {
        return -1;
      }
    },
    _isValidNumber() {
      if (this.modelValue) {
        let val = this._getFullNumber().trim(),
          countryCode = this.currentCountry.code.toUpperCase();
        return isValidNumber(val, countryCode);
      }

      return false;
    },
  }
};
</script>



