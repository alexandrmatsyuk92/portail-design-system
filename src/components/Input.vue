<!--
  Input with an ability to set icon and validation

  Usage:

    <Input
      :prop=''
    />

  Properties:

    // TODO

  Events:

    // TODO
-->
<template>
  <div class="input-component-wrapper">
    <div v-if="topTitle" class="top-title">{{topTitle}}</div>
    <div class="input-field-wrapper">
      <div class="inner-wrapper">
        <Icon
          v-if="icon"
          class="icon-left"
          :source="icon"
          :color="COLORS['gray-400']"
        />
        <input
          :type="type"
          :placeholder="placeholder"
          :disabled="disabled"
          :class="{
          'has-icon': icon,
          'has-icon-right': iconRight,
          'error': !fieldValid
        }"
          v-model="inputValue"
        ></input>
        <Icon
          v-if="rightIcon"
          class="icon-right"
          :source="rightIcon"
          :color="COLORS['gray-400']"
        />
      </div>
      <div
        class="icon-help"
        @mouseover="tooltipVisible = true"
        @mouseleave="tooltipVisible = false"
      >
        <Icon
          v-if="help"
          class="icon-help"
          source="help"
          :color="!tooltipVisible ? COLORS['gray-400'] : COLORS['black']"
        />
        <Tooltip v-html="help" :visible="tooltipVisible"></Tooltip>
      </div>
    </div>
    <ul class="error-list" v-if="!fieldValid">
      <li class="error-message" v-for="error in errors">
        {{ error }}
      </li>
    </ul>
  </div>
</template>

<script>
import Icon from './Icon'
import Tooltip from './Tooltip'
import {COLORS} from '../styles/vars'

export default {
  name: "Input",
  props: {
    placeholder: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'text'
    },
    topTitle: String,
    disabled: Boolean,
    icon: String,
    iconRight: String,
    help: String,
    validators: Array,
    tooltipVisible: Boolean
  },
  components: {Tooltip, Icon},
  data: () => ({
    COLORS,
    inputValue: '',
    errors: []
  }),
  computed: {
    rightIcon () {
      return this.disabled ? 'lock' : this.iconRight
    },
    fieldValid () {
      this.errors = []
      this.validators.forEach((validator) => {
        !validator.validator(this.inputValue) && this.errors.push(validator.message)
      })
      return this.errors.length === 0
    }
  },
  watch: {
    inputValue (newValue) {
      const emittedObject = {
        value: newValue,
        error: !this.fieldValid
      }
      this.$emit('updated:input', emittedObject)
    }
  }
}
</script>

<style lang="less" scoped>
@import '../styles/vars';

.input-component-wrapper {
  display: inline-block;
  position: relative;

  .input-field-wrapper {
    display: flex;
    align-items: center;

    .icon-help {
      position: relative;

      .icon {
        cursor: pointer;
      }
    }

    .inner-wrapper {
      display: inline-block;
      position: relative;

      input {
        font-size: 16px;
        padding: 10px;
        border-radius: @input-border-radius;
        border: 1px solid @color-gray-200;
        color: @color-black;
        width: 100%;
        box-sizing: border-box;

        &.has-icon {
          padding-left: 40px;
        }

        &.has-icon-right {
          padding-right: 40px;
        }

        &.error {
          border-color: @color-red;
        }

        &:focus:not(.error) {
          border-color: @color-brand;
        }

        &:focus {
          outline: none;
        }

        &::placeholder {
          color: @color-gray-500;
        }
      }

      .icon-left,
      .icon-right {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
      }

      .icon-left {
        left: 8px;
      }

      .icon-right {
        right: 8px;
      }
    }
  }

  .error-list {
    margin: 0;
    list-style: none;
    color: @color-red;
    font-size: 14px;
    padding-left: 10px;
  }

  .top-title {
    color: @color-gray-500;
    font-size: 14px;
    line-height: 2;
  }
}
</style>