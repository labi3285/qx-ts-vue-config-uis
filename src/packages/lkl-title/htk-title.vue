<template>
  <div class="lkl-title">
    <div v-if="dotted" class="lkl-title-dot"></div>
    <div class="lkl-title-text" :style="{ color: dotted ? 'var(--clrT1)' : 'var(--clrT2)' }">{{ title }}</div>
    <div v-if="value" class="lkl-title-value">{{ value }}</div>
    <div class="lkl-title-flex-space"></div>
    <slot />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'

@Component
export default class QXTitle extends Vue {
  @Prop({ default: '' }) title!: string;
  @Prop({ default: undefined }) value!: string | number;
  @Prop({ default: true }) dotted!: boolean;
}
</script>

<style lang="less">
@import "../utils/style.less";

.lkl-title {
  margin: var(--marginTB) var(--marginLR) var(--marginTB) var(--marginLR);
  display: flex;
  align-items: center;
  &-dot {
    width: 5px;
    height: 16px;
    .auto-bg-image('htk-dot');
    margin-right: 8px;
  }
  &-text {
    font-weight: bold;
    font-size: var(--font16);
  }
  &-value {
    margin-left: 30px;
    font-weight: bold;
    font-size: 22px;
    color: var(--clrT1);
  }
  &-flex-space {
    flex: 1;
  }
}
</style>
