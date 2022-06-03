<template>
  <div class="todolist__container">
    <div class="todolist__card">
      <div class="todolist__input-block">
        <input
          placeholder="State Your task"
          class="todolist__input"
          type="text"
          v-model="inputValue"
        />
        <button class="todolist__btn" @click="addTask(inputValue)">ADD</button>
      </div>
      <div class="todolist__progress-container">
        <div
          :style="{ width: `${calculateProgress()}%` }"
          class="todolist__progress-unit"
        ></div>
      </div>
      <div>
        <div
          class="todolist__task"
          :key="index"
          v-for="(task, index) in filterByCompletion"
        >
          <input type="checkbox" v-model="task.completed" />
          <span v-if="task.completed">
            <s>{{ task.title }}</s>
          </span>
          <span v-if="!task.completed">
            {{ task.title }}
          </span>
          <button class="todolist__task-remove-btn" @click="removeTask(task)">
            X
          </button>
        </div>
      </div>

      <div class="todolist__filter-btn-container">
        <button
          class="todolist__btn todolist__btn--all"
          @click="
            {
              changeFilter('all');
            }
          "
        >
          ALL
        </button>
        <button
          class="todolist__btn todolist__btn--inprogress"
          @click="
            {
              changeFilter('incomplete');
            }
          "
        >
          IN PROGRESS
        </button>
        <button
          class="todolist__btn todolist__btn--completed"
          @click="
            {
              changeFilter('completed');
            }
          "
        >
          COMPLETED
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export type Task = {
  title: string;
  completed: false;
};

export default defineComponent({
  name: "ToDolist",
  data: () => ({
    inputValue: "",
    filter: "all",
    tasks: [
      {
        title: "saņemies!",
        completed: false,
      },
    ],
  }),
  methods: {
    addTask(input: string) {
      if (input.length > 0) {
        this.tasks.push({
          title: input,
          completed: false,
        }),
          (this.inputValue = "");
      }
    },
    removeTask(task: Task) {
      this.tasks.splice(this.tasks.indexOf(task), 1);
    },
    changeFilter(filterValue: string) {
      this.filter = filterValue;
    },
    calculateProgress: function (): number {
      let completed = 0;
      this.tasks.forEach((task) => {
        if (task.completed) {
          completed++;
        }
      });
      return (completed / this.tasks.length) * 100;
    },
  },
  computed: {
    filterByCompletion() {
      return this.tasks.filter((task) => {
        if (this.filter === "all") {
          return true;
        } else if (this.filter === "completed") {
          return task.completed;
        } else if (this.filter === "incomplete") {
          return !task.completed;
        }
      });
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "./toDoList.scss";
</style>
