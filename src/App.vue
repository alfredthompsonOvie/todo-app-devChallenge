<template>
	<div class="todo__container">
		<h1 class="title">#todo</h1>

		<tab-container 
		:activeBtn="activeBtn" 
		@tab="changeTab"
		/>

		<app-input v-model="todo"/>


		<div class="output__container">
			<ul class="todo__lists">
				<transition-group 
				name="fade" 
				mode="out-in"
				appear>
					<li 
					class="todo__item" 
					v-for="todo in filteredTodos" 
					:key="todo.id">
					<label for="">
						<input
							type="checkbox"
							class="form__control--checkbox input__checkbox"
							:class="{'input__checkbox--completed': todo.isCompleted }"
							@click="todo.isCompleted = !todo.isCompleted"
							v-model="todo.isCompleted"
						/>
					</label>

						<span
							class="todo__content"
							:class="{ 'todo__content--complete': todo.isCompleted }"
						>
							{{ todo.content }}
						</span>

						<button 
						class="deleteBtn" 
						v-show="activeBtn === 'completed'"
						@click="deleteTodo(todo.id)">
							<img src="@/assets/delete_b.svg" alt="" />
							<!-- <span> delete </span> -->
						</button>
					</li>
				</transition-group>
			</ul>

			<app-button
			class="deleteAllBtn"
			@click="clearComplete"
			v-show="showDeleteAllBtn"
			>
				<!-- <span class="material-icons-outlined">delete_outline</span> -->
				<img src="@/assets/delete_b.svg" alt="" />
				<span>delete all</span>
			</app-button>
		</div>
	</div>
</template>

<script>
import TabContainer from "@/components/TabContainer.vue";
import AppInput from "@/components/AppInput.vue";
import AppButton from "@/components/AppButton.vue";

import {v4 as uuidv4} from 'uuid';
import { ref, computed, watch, onMounted } from "vue";

export default {
	name: "App",
	components: {
		TabContainer,
		AppInput,
		AppButton,
	},
	setup() {
		let activeBtn = ref("all");
		let todo = ref("");
		const todos = ref([]);

		let showDeleteAllBtn = computed(() => {
			return (
				activeBtn.value === "completed" &&
				todos.value.filter((todo) => {
					return todo.isCompleted;
				}).length > 0
			);
		});

		watch(todo, (newVal)=>{
			if (todo.value !== "") {
				let item = {
					content: newVal,
					isCompleted: false,
					id: uuidv4(),
				};
				todos.value.unshift(item);
				todo.value = "";
			}
		}, {deep: true})

		watch(todos, (newVal)=>{
				localStorage.setItem("todos", JSON.stringify(todos.value))
				console.log(newVal);
		}, {deep: true})

		onMounted(()=>{
			todos.value = JSON.parse(localStorage.getItem("todos")) || []
		})

		const filteredTodos = computed(() => {
			if (activeBtn.value === "active") {
				return todos.value.filter((t) => {
					return t.isCompleted === false;
				});
			} else if (activeBtn.value === "completed") {
				return todos.value.filter((t) => {
					return t.isCompleted === true;
				});
			} else {
				return todos.value;
			}
		});

		function changeTab(switchTabTo){
			activeBtn.value = switchTabTo
		}


		// function addTodo() {
		// 	if (todo.value !== "") {
		// 		let item = {
		// 			content: todo.value,
		// 			isCompleted: false,
		// 			id: uuidv4(),
		// 		};
		// 		todos.value.unshift(item);
		// 		todo.value = "";
		// 		console.log(todos.value);
		// 	}
		// }
		function clearComplete() {
			console.log("clearComplete");
			todos.value = todos.value.filter(t =>  {
				console.log(t.isCompleted);
				return t.isCompleted !== true
				}
				);
		}
		function deleteTodo(id) {
			console.log(id);
			todos.value = todos.value.filter(t=> {
				return t.id !== id
				})
		}

		return {
			activeBtn,
			todo,
			filteredTodos,
			// addTodo,
			showDeleteAllBtn,

			changeTab,
			clearComplete,
			deleteTodo,
		};
	},
};
</script>

<style></style>
