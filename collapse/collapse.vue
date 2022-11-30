<template>
  <div class="collapse">
    <div class="inner" ref="innerRef" :style="{ height: innerHeight }">
      <slot>
        <span ref="contentRef">江湖路漫漫，其修远兮！</span>
      </slot>
    </div>
    <div class="btn" @click="handleClick" ref="btnRef">
      <i class="icon el-icon-caret-bottom" ref="iconRef"></i>
      <span class="btn-text">{{ btnText }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ymcollapse',
  props: {
    'btn-text': {
      type: String,
      default: '查看详情'
    }
  },
  mounted() {
    this.getSlotsContHeight()
  },
  data() {
    return {
      innerHeight: 0,
      slotsHeight: 0,
      isCollapse: true
    }
  },
  methods: {
    // 点击事件
    handleClick() {
      if (this.innerHeight) {
        this.isCollapse = true
        this.innerHeight = 0
      } else {
        this.isCollapse = false
        this.innerHeight = this.slotsHeight + 'px'
      }

      this.$emit('collapse-btn-click', { isCollapse: this.isCollapse })
    },

    // 获取inner区域的高度
    getSlotsContHeight() {
      const defaultSlots = this.$slots.default
      if (!defaultSlots) {
        this.slotsHeight = this.getFullHeight(this.$refs.contentRef)
        return
      }

      this.slotsHeight = defaultSlots.reduce((acc, curr) => {
        return (acc += this.getFullHeight(curr.elm))
      }, 0)
    },
    // 根据元素获取计算之后的高度
    getFullHeight(el) {
      const computedStyle = window.getComputedStyle(el, null)
      const marginT = computedStyle['margin-top'].split('px')[0] * 1
      const marginB = computedStyle['margin-bottom'].split('px')[0] * 1
      const paddingT = computedStyle['padding-top'].split('px')[0] * 1
      const paddingB = computedStyle['padding-bottom'].split('px')[0] * 1
      const heigth = computedStyle['height'].split('px')[0] * 1

      const resultHeigth = marginB + marginT + paddingT + paddingB + heigth
      return isNaN(resultHeigth) ? 30 : resultHeigth
    }
  },
  watch: {
    isCollapse() {
      const iconRef = this.$refs.iconRef
      const btnRef = this.$refs.btnRef
      if (this.isCollapse) {
        btnRef.style.color = '#000'
        iconRef.style.cssText = `transform: none`
      } else {
        btnRef.style.color = '#409eff'
        iconRef.style.cssText = `transform: rotate(180deg);`
      }
    }
  }
}
</script>

<style scoped>
.collapse {
  overflow: hidden;
  border-radius: 8px;
  box-shadow: 1px 1px 10px #ebebeb;
}
.inner {
  height: 0;
  overflow: hidden;
  transition: all 0.3s linear;
}
.btn {
  font-size: 13px;
  padding: 8px;
  user-select: none;
  cursor: pointer;
  text-align: center;
  border: 1px solid #ebebeb;
  background-color: #fff;
  transition: all 0.1s linear;
}
.btn:hover {
  color: #409eff !important;
  background-color: #f9fafc;
}
.icon {
  transition: all 0.1s linear;
}
</style>
