<template>
  <div>
    <h3>Todos</h3>
    <div class="legend">
      <span>Double click to mark as complete</span>
      <span>
        <span class="incomplete-box"></span> = Incomplete
      </span>
      <span>
        <span class="complete-box"></span> = Complete
      </span>
    </div>
    <div class="legend">
      <span>Right click to edit a title</span>
    </div>
    <div class="todos">
      <div
        class="todo"
        v-for="todo in allTodos"
        :key="todo.id"
        v-bind:class="{'is-complete':todo.completed}"
        @contextmenu.prevent="rClick(todo)"
        @dblclick="onDBLClick(todo)"
      >
        {{todo.title}}
        <i @click="deleteTodo(todo.id)" class="fas fa-trash-alt"></i>
      </div>
      <Modal ref="modal"></Modal>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import Modal from "./Modal";

export default {
  name: "Todos",
  methods: {
    ...mapActions(["fetchTodos", "deleteTodo", "updateTodo"]),
    onDBLClick(todo) {
      const updTodo = {
        id: todo.id,
        title: todo.title,
        completed: !todo.completed
      };
      this.updateTodo(updTodo);
    },
    rClick(todo) {
      this.$refs.modal.title = todo.title;
      this.$refs.modal.id = todo.id;
      this.$refs.modal.completed = todo.completed;
      let element = this.$refs.modal.$el;
      // eslint-disable-next-line
      $(element).modal("show");
    }
  },
  computed: mapGetters(["allTodos"]),
  created() {
    this.fetchTodos();
  },
  components: {
    Modal
  }
};
</script>

<style scoped>
.todos {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 1rem;
}
.todo {
  border: 1px solid #ccc;
  background: #41b883;
  padding: 1rem;
  border-radius: 5px;
  text-align: center;
  position: relative;
  cursor: pointer;
}
i {
  position: absolute;
  bottom: 10px;
  right: 10px;
  color: #fff;
  cursor: pointer;
}
.legend {
  display: flex;
  justify-content: space-around;
  margin-bottom: 1rem;
}
.complete-box {
  display: inline-block;
  width: 10px;
  height: 10px;
  background: #35495e;
}
.incomplete-box {
  display: inline-block;
  width: 10px;
  height: 10px;
  background: #41b883;
}
.is-complete {
  background: #35495e;
  color: #fff;
}
@media (max-width: 500px) {
  .todos {
    grid-template-columns: 1fr;
  }
}
</style>
