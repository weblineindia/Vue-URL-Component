<template>
  <div>
    <input
      :id="id"
      v-model="value"
      v-validate.immediate="{ regex: validationRegx }"
      :name="name"
      type="url"
      :class="{
        'text-visible': value ? true : false,
        'error-text': errors.has(name),
        'is-danger': errors.has(name),
        'form-control': true,
        'mt-4':  value  ? false : true
      }"
      :placeholder="placeholder"
      :tabindex="tabindex"
      autocomplete="off"
      :disabled="disabled"
      @focus="onFocus($event)"
      @input="onChangeField($event)"
      @blur="onBlur($event)"
      @keypress.enter.prevent
    />
  </div>
</template>
<script>
import Regex from "./regex";
import Vuelidate from "vuelidate";
import Vue from "vue";
import * as VeeValidate from "vee-validate";
Vue.use(Vuelidate);
Vue.use(VeeValidate, {
  events: "change|blur"
});
export default {
  props: {
    value: {
      type: String,
      default: ""
    },
    socialType: {
      type: String,
      value: "FACEBOOK"
    },
    validationRegx: {
      type: RegExp,
      default: ""
    },
    tabindex: {
      type: Number,
      default: 1
    },
    placeholder: {
      type: String,
      default: "facebookUrl"
    },
    id: {
      type: String,
      default: "facebookUrl"
    },
    name: {
      type: String,
      default: "facebookUrl"
    },
    hide: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    /**on focus url field */
    onFocus(event) {
      this.$emit("focus", event);
    },
    /** on blur url field */
    onBlur() {
      this.$emit("onBlur", event, this.placeholder, this.errors);
    },
    /**
     *  On url change event
     */
    onChangeField() {
      this.$validator.reset();

      if (this.value.trim() === "") {
        this.value = this.value
          .replace(Regex.ALPHABATICS_NUMERICS_REGEX_WITHOUT_SPACE, "")
          .trim();
      }
      this.$emit("onChange", this.value, this.id, this.tabindex, this.errors);
    }
  }
};
</script>
<style scoped>
@import './style.css'
</style>
