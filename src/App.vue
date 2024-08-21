<script setup>
import { computed, onMounted, ref, watch } from "vue";

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: Date.now(),
  });
  input_content.value = "";
  input_category.value = null;
};

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  }),
);

const toggleEdit = (todo) => {
  todo.isEditing = !todo.isEditing;
};

const removeItem = (todo) => {
  todos.value = todos.value.filter((item) => item !== todo);
};

watch(
  todos,
  (newItems) => {
    localStorage.setItem("todos", JSON.stringify(newItems));
  },
  { deep: true },
);

watch(name, (newName) => {
  localStorage.setItem("name", newName);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Bonjour,
        <input v-model="name" placeholder="Ecrire votre nom ici" type="text" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>CREER MA TODOLIST</h3>
      <form @submit.prevent="addTodo">
        <h4>Que voulez vous ajouter à votre liste ?</h4>
        <input
          v-model="input_content"
          placeholder="ex: Faire les courses"
          type="text"
        />
        <h4>Choisir une categorie</h4>
        <div class="options">
          <label>
            <input
              id="category"
              v-model="input_category"
              name="category"
              type="radio"
              value="travail"
            />
            <span class="bubble business"></span>
            <div>Travail</div>
          </label>
          <label>
            <input
              id="category"
              v-model="input_category"
              name="category"
              type="radio"
              value="personnel"
            />
            <span class="bubble personal"></span>
            <div>Personnel</div>
          </label>
        </div>
        <input type="submit" value="Ajouter à ma liste" />
      </form>
    </section>
    <section class="todo-list">
      <h3>LISTE DES TACHES</h3>
      <div class="list">
        <div
          v-for="todo in todos_asc"
          key="todo.createdAt"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input v-model="todo.done" type="checkbox" />
            <span
              :class="`bubble ${todo.category === 'travail' ? 'business' : 'personal'}`"
            ></span>
          </label>
          <div class="todo-content">
            <input
              v-if="todo.isEditing"
              v-model="todo.content"
              type="text"
              @blur="toggleEdit(todo)"
            />
            <span v-else>{{ todo.content }}</span>
          </div>
          <div class="actions">
            <button class="edit" @click="toggleEdit(todo)">Editer</button>
          </div>
          <div class="actions">
            <button class="delete" @click="removeItem(todo)">Supprimer</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
