<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addNewTask"
        class="col"
        square
        filled
        dense
        bg-color="white"
        placeholder="Add task"
      >
        <template v-slot:append>
          <q-btn round flat dense icon="add" @click.prevent="addNewTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" bordered separator>
      <q-item
        tag="label"
        v-for="(task, index) in tasks"
        :key="index"
        @click.prevent="task.done = !task.done"
        :class="{ 'done bg-blue-1' : task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox color="primary" class="no-pointer-events" v-model="task.done" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn flat round dense color="primary" icon="delete" @click.prevent="confirm(index)" />
        </q-item-section>
      </q-item>
    </q-list>

    <div class="no-tasks absolute-center" v-if="!tasks.length">
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      newTask: "",
      tasks: [
        {
          title: "Code",
          done: false
        },
        {
          title: "Eat",
          done: false
        },
        {
          title: "Sleep",
          done: false
        }
      ]
    };
  },

  methods: {
    markAsDone(task) {
      task.done = !task.done;
    },

    confirm(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Would you like to delete task?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.deleteTask(index);
          this.showNotif();
        });
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    showNotif() {
      this.$q.notify({
        progress: true,
        message: "Task deleted.",
        color: "#263238"
      });
    },

    addNewTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      });
      this.newTask = "";
    }
  }
};
</script>

<style lang="scss" scoped>
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.3;
}
</style>
