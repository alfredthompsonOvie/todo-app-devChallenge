<template>
<div class="tab__container">
	<app-button
		class="btn btn--all"
		@click="updateTab('all')"
		:class="activeBtn === 'all' ? 'active' : ''"
		>all</app-button
	>
	<app-button
		class="btn"
		@click="updateTab('active')"
		:class="activeBtn === 'active' ? 'active' : ''"
		>active</app-button
	>
	<app-button
		class="btn"
		@click="updateTab('completed')"
		:class="activeBtn === 'completed' ? 'active' : ''"
		>completed</app-button
	>
</div>
</template>

<script>
import AppButton from "./AppButton.vue";
export default {
	name: "TabContainer",
	components: {
		AppButton,
	},
  props: {
    activeBtn: {
      type: String,
      required: true
    }
  },
  emits: ['tab'],

  setup(props, context){
    function updateTab(content){
      context.emit('tab', content)
    }

    return {
      updateTab
    }
  }
};
</script>

<style></style>
