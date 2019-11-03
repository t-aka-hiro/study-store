<template>
  <v-layout justify-center>
    <v-flex md8 xs12>
      <v-card justify-center align-center class="container">
        <v-card-title>Task</v-card-title>
        <p>
          <v-text-field v-model="content" label="Type here!" name="content" @focus="set_flg"></v-text-field>
        </p>
        <div>
          <v-btn @click="insert">save</v-btn>
          <v-btn @click="find">find</v-btn>
        </div>
        <v-list flat>
          <v-list-item-group color="primary">
            <v-list-item v-for="(todo, index) in display_todos" :key="index">
              <span>{{ todo.content }}</span>
              <v-spacer />
              <span>({{ todo.created }})</span>
              <v-spacer />
              <span @click="remove(todo)">×</span>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
    </v-flex>
  </v-layout>
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
