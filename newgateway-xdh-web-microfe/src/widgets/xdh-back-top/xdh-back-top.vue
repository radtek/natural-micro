<template>
  <div :class="classes" :style="styles" @click="back">
    <slot>
      <div :class="innerClasses">
        <i class="el-icon-arrow-up"></i>
      </div>
    </slot>
  </div>
</template>
<script>
  import {scrollTop} from '../../utils/dom';
  import {on, off} from 'element-ui/lib/utils/dom';

  const prefixCls = 'xdh-back-top';

  /**
   * XdhBackTop
   * @module widgets/xdh-back-top
   */
  export default {
    name: 'XdhBackTop',
    /**
     * props
     * @member
     * @property {object} props
     * @property {number} [props.height=400] 页面滚动高度达到该值时才显示组件
     * @property {number} [props.bottom=30] 组件距离底部的距离
     * @property {number} [props.right=30] 组件距离右部的距离
     * @property {number} [props.duration=1000] 滚动动画持续时间，单位 毫秒
     */
    props: {
      height: {
        type: Number,
        default: 400
      },
      bottom: {
        type: Number,
        default: 30
      },
      right: {
        type: Number,
        default: 30
      },
      duration: {
        type: Number,
        default: 1000
      }
    },
    data() {
      return {
        backTop: false
      };
    },
    mounted() {
      on(window, 'scroll', this.handleScroll);
      on(window, 'resize', this.handleScroll);
    },
    beforeDestroy() {
      off(window, 'scroll', this.handleScroll);
      off(window, 'resize', this.handleScroll);
    },
    computed: {
      classes() {
        return [
          `${prefixCls}`,
          {
            [`${prefixCls}-show`]: this.backTop
          }
        ];
      },
      styles() {
        return {
          bottom: `${this.bottom}px`,
          right: `${this.right}px`
        };
      },
      innerClasses() {
        return `${prefixCls}-inner`;
      }
    },
    methods: {
      handleScroll() {
        this.backTop = window.pageYOffset >= this.height;
      },
      back() {
        const sTop = document.documentElement.scrollTop || document.body.scrollTop;
        scrollTop(window, sTop, 0, this.duration);
        /**
         * 点击事件
         * @event module:widgets/xdh-back-top#click
         */
        this.$emit('click');
      }
    }
  };
</script>

