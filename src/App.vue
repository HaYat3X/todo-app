<script lang="ts" setup>
import { ref, onMounted } from "vue";
const task = ref();
const priority = ref();
const prompt = ref(false);
let tasks: any = [];

onMounted(async () => {
  const tasksJson = localStorage.getItem("tasks");

  if (tasksJson) {
    tasks = JSON.parse(tasksJson);
  }
});

const addTask = () => {
  const tasksJson = localStorage.getItem("tasks");
  tasks = tasksJson ? JSON.parse(tasksJson) : [];

  // 新しいタスクを追加
  tasks.push({ task: task.value, priority: priority.value });
  localStorage.setItem("tasks", JSON.stringify(tasks));

  location.reload();
};

const deleteTask = (index: number) => {
  tasks.splice(index, 1);
  localStorage.setItem("tasks", JSON.stringify(tasks));
  location.reload();
};

const options = ["A", "B", "C", "D", "E"];
</script>

<template>
  <q-layout view="hHh lpR fff">
    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-toolbar-title> TodoLists </q-toolbar-title>

        <q-btn
          outline
          round
          color="white"
          size="sm"
          icon="add"
          @click="prompt = true"
        />

        <q-dialog v-model="prompt" persistent>
          <q-card style="min-width: 350px">
            <q-card-section>
              <div class="text-h6">Add Task</div>
            </q-card-section>

            <q-card-section class="q-pt-none">
              <q-input v-model="task" label="Task" />
              <q-select
                v-model="priority"
                :options="options"
                label="Priority"
              />
            </q-card-section>

            <q-card-actions align="right" class="text-primary">
              <q-btn flat label="Cancel" v-close-popup />
              <q-btn flat label="Add Task" @click="addTask" />
            </q-card-actions>
          </q-card>
        </q-dialog>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <q-page class="q-pa-md">
        <q-markup-table>
          <thead>
            <tr>
              <th class="text-left">Task</th>
              <th class="text-center">Priority</th>
              <th class="text-right">Action</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
              <td class="text-left">{{ task.task }}</td>
              <td class="text-center">
                <q-badge v-if="task.priority === 'A'" color="negative">
                  {{ task.priority }}
                </q-badge>
                <q-badge v-if="task.priority === 'B'" color="primary">
                  {{ task.priority }}
                </q-badge>
                <q-badge v-if="task.priority === 'C'" color="secondary">
                  {{ task.priority }}
                </q-badge>
                <q-badge v-if="task.priority === 'D'" color="accent">
                  {{ task.priority }}
                </q-badge>
                <q-badge v-if="task.priority === 'E'" color="dark">
                  {{ task.priority }}
                </q-badge>
              </td>

              <td class="text-right">
                <q-btn
                  flat
                  round
                  color="red"
                  icon="delete"
                  size="sm"
                  @click="deleteTask(index)"
                />
              </td>
            </tr>
          </tbody>
        </q-markup-table>
      </q-page>
    </q-page-container>

    <q-footer elevated class="bg-dark text-white text-center q-py-xs">
      2024 Takeda Hayate
    </q-footer>
  </q-layout>
</template>

<style></style>
