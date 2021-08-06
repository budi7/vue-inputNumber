<template>
  <input
    :disabled="disabled"
    autocomplete="off"
    type="text"
    :class="`form-control ${inputClass}`"
    :value="input"
    @keypress="validate($event)"
    @input="updateValue($event.target.value)"
    @focus="handleFocus(true, $event.target.value)"
    @blur="handleFocus(false, $event.target.value)"
  >
</template>

<script>
export default {
  props: {
    value: {
      type: Number,
      default: null
    },
    maxValue: {
      type: Number,
      default: null
    },
    minValue: {
      type: Number,
      default: null
    },
    disabled: {
      type: Boolean,
      default: false
    },
    inputClass: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      isFocus: false
    }
  },
  computed: {
    input: {
      set (val) {
        return val
      },
      get () {
        if (!this.value) {
          return null
        }

        const reg = /^[0-9]+$/
        if (reg.test(this.value)) {
          let val = (this.value / 1).toFixed(0).toString().replace('.', ',')
          val = val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.')
          return val
        } else {
          return this.value.toString().replace(/\D/g, '')
        }
      }
    }
  },
  mounted () {
    this.input = this.value ? this.value : null
  },
  methods: {
    validate (evt) {
      evt = (evt) || window.event
      const charCode = (evt.which) ? evt.which : evt.keyCode
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        evt.preventDefault()
      } else {
        if (this.minValue && (parseInt(this.minValue) >= 0)) {
          if (parseInt(this.value) <= this.minValue) {
            evt.preventDefault()
            this.input = this.minValue
          }
        }

        if (this.maxValue && (parseInt(this.maxValue) >= 0)) {
          if (parseInt(this.value) >= this.maxValue) {
            evt.preventDefault()
            this.input = this.maxValue
          }
        }

        return true
      }
    },
    updateValue (val) {
      const res = val.split('.').join('')
      this.$emit('input', res ? parseInt(res) : null)
    },
    handleFocus (state, val) {
      if (state) {
        this.$emit('focus')
      } else {
        this.$emit('blur')
        const res = val.split('.').join('')
        this.$emit('change', res ? parseInt(res) : null)
      }
    }
  }
}
</script>
