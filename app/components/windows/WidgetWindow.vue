<template>
<modal-layout
  :title="windowTitle"
  :showControls="false"
  :customControls="true"
  :fixedSectionHeight="254"
  v-if="previewSource">

  <div slot="fixed">
    <display class="display" :sourceId="previewSource.id" @click="createProjector"/>
    <tabs ref="tabs" :tabs="tabsList" :value="value" @input="value => $emit('input', value)"></tabs>
  </div>

  <div slot="content" class="content" v-if="canRender">

    <!-- browser-source properties tab -->
    <div v-if="value === 'source'">
      <GenericForm v-model="properties" @input="onPropsInputHandler"/>
    </div>

    <!-- other tabs -->
    <div v-for="tabItem in tabsList" :key="tabItem.value">
      <div v-if="tabItem.value !== 'source'">
        <slot :name="tabItem.value" v-if="tabItem.value === value"></slot>
      </div>
    </div>
  </div>

  <!-- buttons -->
  <div slot="controls" v-if="canRender">
    <div v-for="tabItem in tabsList" :key="tabItem.value" v-if="tabItem.value === value">
      <slot :name="tabItem.value + '-controls'" v-if="(tab && tab.showControls) || tabItem.value === 'source'">
        <button
            class="button button--default"
            @click="close">
          {{ $t('Close') }}
        </button>
      </slot>
    </div>
  </div>

</modal-layout>
</template>

<script lang="ts" src="./WidgetWindow.vue.ts"></script>

<style lang="less" scoped>
  .display {
    height: 200px !important;
    cursor: pointer;
  }
</style>
