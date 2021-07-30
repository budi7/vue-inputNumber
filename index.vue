<template>
  <input
    :disabled="disabled"
    autocomplete="off"
    type="text"
    :class="`form-control ${inputClass}`"
    :value="input"
    @keypress="validate($event)"
    @input="updateValue($event.target.value)"
    @focus="$emit('focus')"
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
      default: null
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
        if (parseInt(this.minValue) >= 0) {
          if (parseInt(this.value) <= this.minValue) {
            evt.preventDefault()
          }
        }

        if (parseInt(this.maxValue) >= 0) {
          if (parseInt(this.value) >= this.maxValue) {
            evt.preventDefault()
          }
        }

        return true
      }
    },
    updateValue (val) {
      const res = val.split('.').join('')
      this.$emit('input', parseInt(res))
    }
  }
}
</script>
