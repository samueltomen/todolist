<script setup>
import { computed, onMounted, ref, watch } from "vue";

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  }),
);
const addTodo = () => {};
watch(name, (newName) => {
  localStorage.setItem("name", newName);
});
onMounted(() => {
  name.value = localStorage.getItem("name") || "";
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
      <h3>MA TODOLIST</h3>
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
  </main>
</template>
