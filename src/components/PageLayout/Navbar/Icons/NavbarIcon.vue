<template>
  <b-nav-dropdown
    right
    class="d-block d-md-inline-block dropdown"
    size="lg"
    variant="link"
    toggle-class="px-0 text-decoration-none"
    no-caret
    @show="onShow"
  >
    <template #button-content>
      <div>
        <slot name="icon"></slot>
        <span class="d-md-none">
          <slot name="text"></slot>
        </span>
      </div>
    </template>
    <!-- The default scope slot has the ability to hide the dropdown: 
    https://bootstrap-vue.org/docs/components/nav#comp-ref-b-nav-item-dropdown-slots -->
    <template v-slot="hideDropdown">
      <slot :hide-dropdown="hideDropdown"></slot>
    </template>
  </b-nav-dropdown>
</template>
<script lang="ts">
  import { Component, Prop, Vue } from 'vue-property-decorator';

  @Component({
    components: {},
  })
  export default class NavbarIcon extends Vue {
    onShow() {
      this.$emit('shown');
    }
  }
</script>

<style lang="scss" scoped></style>
