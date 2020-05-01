<template>
  <div><slot/></div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
declare let DlhSoft: any;

@Component
export default class LoadChart extends Vue {
  @Prop() private items!: any[];
  @Prop() private settings!: any;
  @Prop() private license?: string;

  private mounted() {
    const changeHandler = this.settings.itemPropertyChangeHandler;
    DlhSoft.Controls.LoadChartView.initialize(this.$el, this.items, this.settings, this.license);
    this.settings.itemPropertyChangeHandler = (item: any, propertyName: string, isDirect: boolean, isFinal: boolean) => {
      if (changeHandler)
        changeHandler(item, propertyName, isDirect, isFinal);
      this.$emit('change', {item: item, propertyName: propertyName, isDirect: isDirect, isFinal: isFinal});
    }
  }
}
</script>