<template>
  <v-card justify-center align-center class="container">
    <h1>Task</h1>
    <p>
      <v-text-field v-model="content" label="Type here!" name="content" @focus="set_flg"></v-text-field>
    </p>
    <div>
      <v-btn @click="insert">save</v-btn>
      <v-btn @click="find">find</v-btn>
    </div>
    <ul>
      <li v-for="(todo, index) in display_todos" :key="index">
        <span>{{ todo.content }}</span>
        <span>({{ todo.created }})</span>
        <span @click="remove(todo)">×</span>
      </li>
    </ul>
  </v-card>
</template>

<script>
import { mapState } from "vuex";

export default {
  data: function() {
    return {
      content: "",
      find_flg: false
    };
  },
  computed: {
    ...mapState(["todos"]),
    display_todos: function() {
      if (this.find_flg) {
        var arr = [];
        var data = this.todos;
        data.forEach(element => {
          if (element.content.toLowerCase() == this.content.toLowerCase()) {
            arr.push(element);
          }
        });
        return arr;
      } else {
        return this.todos;
      }
    }
  },
  methods: {
    insert: function() {
      this.$store.commit("insert", { content: this.content });
      this.content = "";
    },
    find: function() {
      this.find_flg = true;
    },
    set_flg: function() {
      if (this.find_flg) {
        this.find_flg = false;
        this.content = "";
      }
    },
    remove: function(todo) {
      this.$store.commit("remove", todo);
    }
  }
};
</script>
