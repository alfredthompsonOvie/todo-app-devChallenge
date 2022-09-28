<template>
	<div class="todo__container">
		<h1 class="title">#todo</h1>

		<div class="tab__container">
			<button
				class="btn btn--all"
				@click="activeBtn = 'all'"
				:class="activeBtn === 'all' ? 'active' : ''"
			>
				all
			</button>

			<button
				class="btn"
				@click="activeBtn = 'active'"
				:class="activeBtn === 'active' ? 'active' : ''"
			>
				active
			</button>
			<button
				class="btn"
				@click="activeBtn = 'completed'"
				:class="activeBtn === 'completed' ? 'active' : ''"
			>
				completed
			</button>
		</div>

		<div class="input__Container">
			<form @submit.prevent="addTodo" class="form__group">
				<label>
					<input type="text" placeholder="add details" v-model="todo" />
				</label>
				<button type="submit" class="addTodo">add</button>
			</form>
		</div>

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

						<button class="deleteBtn" v-show="activeBtn === 'completed'">
							<img src="@/assets/delete_b.svg" alt="" />
							<!-- <span> delete </span> -->
						</button>
					</li>
				</transition-group>
			</ul>
			<button class="deleteAllBtn" v-show="showBtn">
				<!-- v-show="activeBtn === 'completed' && activeBtn.length > 0" -->
				<!-- <span class="material-icons-outlined">delete_outline</span> -->
				<img src="@/assets/delete_b.svg" alt="" />
				<span>delete all</span>
			</button>
		</div>
	</div>
</template>

<script>
import { ref, computed } from "vue";
import {v4 as uuidv4} from 'uuid';
export default {
	setup() {
		let activeBtn = ref("all");
		let todo = ref("");
		const todos = ref([]);

		let showBtn = computed(() => {
			return (
				activeBtn.value === "completed" &&
				todos.value.filter((todo) => {
					return todo.isCompleted;
				}).length > 0
			);
		});

		console.log(activeBtn.value.length);

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

		// onMounted(()=>{
		//   fetch('https://syncwith.com/api/metaweather-api', {
		//     method : "GET",
		//     mode: 'cors',
		// }).then(res=> res.json()).then(data=> console.log(data))
		// })

		// watch(todo, (newVal)=>{
		//   if(newVal !== ''){
		//     let item = {
		//       content: newVal,
		//       isCompleted: false,
		//       id: 1
		//     }
		//     todos.value.unshift(item)
		//   }
		// })

		function addTodo() {
			if (todo.value !== "") {
				let item = {
					content: todo.value,
					isCompleted: false,
					id: uuidv4(),
				};
				todos.value.unshift(item);
				todo.value = "";
				console.log(todos.value);
			}
		}

		return {
			activeBtn,
			todo,
			filteredTodos,
			addTodo,
			showBtn,
		};
	},
};
</script>

<style></style>
