<template>
  <div class="lkl-haotk-infos">
    <div class="lkl-haotk-infos-card">
      <div v-if="help" class="lkl-haotk-infos-card-help"></div>
      <slot />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'

@Component
export default class LklHaotkInfosCard extends Vue {
  @Prop({ default: false }) help!: boolean;
}
</script>

<style lang="less">
@import "../utils/style.less";

.lkl-haotk-infos {
  width: 100%;
  margin-top: var(--marginTB);
  margin-bottom: var(--marginTB);
  &-card {
    margin: 0 var(--marginLR) 0 var(--marginLR);
    width: calc(100% - var(--marginLR) * 2);
    .auto-bg-image('resources/haotk-infos-bg');
    background-size: 100% !important;
    border-radius: 8px;
    position: relative;
    overflow: hidden;
    background-color: #ffd53e;
    &-help {
      width: 17px;
      height: 17px;
      .auto-bg-image('resources/haotk-help');
      position: absolute;
      top: 12px;
      right: 12px;
    }
  }
}
</style>
