<template>
  <div ref="loadMore" v-frameChange="onViewChange" v-offsetChange="onViewChange" class="lkl-load-more">
    <slot>
      <div class="lkl-load-more-slot-default">{{ isLoading ? '加载中...' : isThereMore ? '已加载' : '没有更多' }}</div>
    </slot>
  </div>
</template>

<script lang="ts">
import { Component, Watch, Prop, Vue } from 'vue-property-decorator'
import frameChange from '../utils/directives/frameChange'
import offsetChange from '../utils/directives/offsetChange'

@Component({
  directives: {
    frameChange,
    offsetChange
  }
})
export default class LklLoadMore extends Vue {
  @Prop({ required: true }) isLoading!: boolean;
  @Prop({ default: false }) isLoadOnMounted!: boolean;
  @Prop({ default: true }) isThereMore!: boolean;

  @Watch('isLoading')
  private onIsLoadingChange () {
    if (!this.isLoading && this.isThereMore) {
      this.onViewChange()
    }
  }

  @Watch('isThereMore')
  private onIsThereMore () {
    if (!this.isLoading && this.isThereMore) {
      this.onViewChange()
    }
  }

  private mounted () {
    if (this.isLoadOnMounted && this.isThereMore) {
      this.loadData()
    }
  }

  private loadData () {
    this.$emit('update:isLoading', true)
    this.$emit('load')
  }

  private onViewChange () {
    const el = this.$refs.loadMore as HTMLDivElement
    let _offsetTop = el.offsetTop
    let _next = el.parentElement
    while (_next) {
      // eslint-disable-next-line @typescript-eslint/ban-ts-comment
      // @ts-ignore
      if (_next.__vue_BScroll_scrollPosition__) {
        // eslint-disable-next-line @typescript-eslint/ban-ts-comment
        // @ts-ignore
        _offsetTop = _next.__vue_BScroll_scrollPosition__.y + _offsetTop
        break
      } else {
        _offsetTop += _next.offsetTop
      }
      _next = _next.parentElement
    }
    let isShow = false
    if (_next) {
      const _tolerance = 50
      const _scroll = _next
      const _centerY = _offsetTop + el.offsetHeight / 2
      const _visibleBottom = _scroll.offsetHeight + _tolerance
      isShow = _centerY > 0 && _centerY < _visibleBottom
    } else {
      const _tolerance = 50 // 部分浏览器在滚动页面的时候会隐藏导航栏从而导致滚动出现误差
      const _visibleBottom = window.scrollY + document.documentElement.clientHeight + _tolerance
      const _visibleTop = window.scrollY
      const _offsetTop = el.offsetTop
      isShow = _offsetTop > _visibleTop && _offsetTop < _visibleBottom
    }
    if (this.isLoading === false && this.isThereMore && isShow) {
      this.loadData()
    }
  }
}
</script>

<style lang="less">
.lkl-load-more {
  width: 100%;
  text-align: center;
  &-slot-default {
    font-size: var(--font14);
    color: var(--clrT3);
    padding-top: var(--marginTB);
    padding-bottom: var(--marginTB);
  }
}
</style>
