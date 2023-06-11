<template>
  <div>
    <header>
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container d-flex justify-content-start">
          <a
            class="navbar-brand"
            href="#"
          >
            TaskList
          </a>
          <a
            class="nav-link"
            href="#"
          >Home</a>
        </div>
      </nav>
    </header>

    <main class="container mt-3">
      <h6>{{ tasks.length ? 'Task' : 'Belum ada task' }}</h6>
      <div class="list-task border-top">
        <div
          v-for="(task, i) of tasks"
          :key="i"
          class="item-task d-flex align-items-start border-bottom pt-3 pb-4"
        >
          <input
            id="task"
            type="checkbox"
            name="status"
            class="me-2 mt-2"
            :checked="task.isDone"
            @change="handleTaskIsDone(task)"
          >
          <div class="d-flex flex-column">
            <div
              :class="{ 'text-decoration-line-through': task.isDone }"
              class="title-task mb-1"
            >
              {{ task.title }}
            </div>
            <div class="description-task small text-muted">
              {{ task.description }}
            </div>
            <a
              class="text-danger"
              @click="handleDeleteTask(task)"
            >Delete</a>
          </div>
        </div>
      </div>

      <div class="action py-2">
        <button
          v-if="!isCreating"
          href="#"
          class="btn btn-info rounded-pill"
          @click="isCreating = !isCreating"
        >
          Add
          Task
        </button>
        <div
          v-else
          class="add-card"
        >
          <div class="card mb-2">
            <div class="card-body d-flex flex-column p-0">
              <input
                v-model="titleValue"
                class="form-control border-0 mb-2"
                placeholder="Title"
                type="text"
              >
              <textarea
                v-model="descriptionValue"
                class="form-control border-0 small"
                placeholder="Description"
                rows="3"
              />
            </div>
          </div>
          <div class="button-wrapper d-flex">
            <button
              class="btn btn-primary me-2"
              @click.prevent="handleOnSubmit"
            >
              Save
            </button>
            <button
              class="btn btn-outline-secondary"
              @click="isCreating = !isCreating"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </main>

    <footer class="bg-light fixed-bottom">
      <p class="container p-1">
        &copy; 2023 TaskList. All right reserved
      </p>
    </footer>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

export default {
  data() {
    return {
      tasks: [
        {
          title: 'Belajar Vue',
          description: 'Materi Syntax Template Dasar',
          isDone: false,
        },
      ],
      isCreating: false,
      titleValue: '',
      descriptionValue: '',
      errorMessage: '',
    };
  },
  methods: {
    handleOnSubmit() {
      if (!this.titleValue || !this.descriptionValue) return alert('Title dan deskripsi tidak boleh kosong');

      const newTask = {
        title: this.titleValue,
        description: this.descriptionValue,
        isDone: false,
      };
      
      this.tasks.push(newTask);
      
      this.titleValue = '';
      this.descriptionValue = '';
      this.isCreating = false;
    },

    handleDeleteTask(selectedTask) {
      this.tasks = this.tasks.filter((task) => task !== selectedTask);
    },

    handleTaskIsDone(selectedTask) {
      this.tasks = this.tasks.map((task) => {
        if (task === selectedTask) {
          return {
            ...task,
            isDone: !task.isDone,
          };
        }
        return task;
      });
    },
  },
};
</script>
