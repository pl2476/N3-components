<template>
  <div :class="`${prefixCls}-btn-group ${prefixCls}-checkbox-group`">
    <slot></slot>
    <validate
      :name="name"
      :rules="rules"
      :custom-validate="customValidate" 
      :current="currentValue">
    </validate>
  </div>
</template>

<script>
import valMixin from '../Mixin/valMixin'
import validate from '../validate'
import events from '../utils/events'

export default {
  name: 'n3CheckboxGroup',
  mixins: [valMixin, events],
  props: {
    value: {
      type: Array,
      default () {
        return []
      }
    },
    type: {
      type: String,
      default: 'checkbox'
    },
    prefixCls: {
      type: String,
      default: 'n3'
    }
  },

  data () {
    return {
      currentValue: this.value
    }
  },

  methods: {
    init () {
      let children = this.$children
      let ret = []
      children.forEach((item) => {
        item.currentChecked ? ret.push(item.label) : ''
      })
      this.currentValue = ret
    }
  },

  watch: {
    value (val) {
      this.currentValue = val
    },
    currentValue (val) {
      this.broadcast('n3Checkbox', 'n3@checkboxgroupChange', val)
      this.broadcast('n3CheckboxBtn', 'n3@checkboxgroupChange', val)
      this.$emit('input', val)
      this.$emit('change', val)
    }
  },

  created () {
    this.$on('n3@checkboxChange', () => {
      this.init()
    })
  },

  mounted () {
    this.$nextTick(() => {
      this.init()
    })
  },

  components: {
    validate
  }

}
</script>