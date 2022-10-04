<template>
	<div class="input__Container">
		<form @submit.prevent="sendtodo" class="form__group">
			<label>
				<input 
				type="text" 
				placeholder="add details" 
				:value="modelValue"
				@input="updateTodoContent"
				ref="input"
				/>
				<!-- @input="todoContent = $event.target.value" -->
			</label>
			<button 
			type="submit" 
			class="addTodo"
			>add</button>
		</form>
	</div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "AppInput",
	props: ["modelValue"],

	setup(props, context){
		let todoContent = ref('')
		const input = ref(null)

		function updateTodoContent(e){
			todoContent.value = e.target.value
		}

		function sendtodo(){
			context.emit('update:modelValue', todoContent.value)
			input.value.value = ''
			todoContent.value = ""
		}

		return {
			sendtodo,
			todoContent,
			updateTodoContent,
			input
		}
	}
};
</script>

<style></style>
