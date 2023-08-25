<template>
    <div class="flex flex-col gap-3 p-3 border-2 border-blue-500 my-5 max-w-[720px] m-auto rounded-lg items-center">
        <h2 class="w-full text-lg text-sky-700 font-semibold text-center">Create a To-Do Task</h2>
        <form @submit.prevent="addTask" class="flex gap-4 w-full">
            <input v-model="newDate" type="date"
                class="min-w-[150px] outline-none border-2 border-gray-400 rounded px-2 flex-1" required />
            <input v-model="newTask" type="text" placeholder="Enter task description" required
                class="shadow appearance-none border rounded w-full max-w-[480px] py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" />
            <button type="submit"
                class="bg-white hover:bg-gray-100 text-gray-800 text-sm font-semibold py-2 px-2 border border-gray-400 rounded shadow min-w-[80px]">Add
                Task</button>
        </form>

        <h2 class="w-full text-lg text-sky-700 font-semibold text-center mt-4">Pending Tasks</h2>
        <ul class="w-full flex flex-col gap-1">
            <li class="flex w-full gap-1 justify-between py-1 items-center border-2 border-gray-300 px-2 rounded"
                v-for="task in pendingTasks" :style="taskStyle(task)" :key="task.id">
                <input id="disabled-checked-checkbox" type="checkbox" @change="readTask(task)"
                    class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 dark:focus:ring-blue-600">
                <span class="text-base text-gray-700">{{ task.description }}</span>
                <span class="text-base">{{ task.workDate }}</span>
                <div class="flex justify-center gap-1">
                    <button class="text-sm text-gray-700 border-2 border-gray-500 rounded p-1" @click="completeTask(task)">
                        <component class="text-green-600" :is="icons[0]"></component>
                    </button>
                    <button class="text-sm text-gray-700 border-2 border-gray-500 rounded p-1" @click="duplicateTask(task)">
                        <component class="text-yellow-600" :is="icons[1]"></component>
                    </button>
                    <button class="text-sm text-gray-700 border-2 border-gray-500 rounded p-1" @click="deleteTask(task)">
                        <component class="text-red-600" :is="icons[2]"></component>
                    </button>
                </div>

            </li>
        </ul>

        <h2 class="w-full text-lg text-sky-700 font-semibold text-center mt-4">Completed Tasks</h2>
        <ul class="w-full flex flex-col gap-1">
            <li class="w-full border-2 border-gray-300 px-2 py-2 rounded" v-for="task in completedTasks" :key="task.id">
                {{ task.description }}
            </li>
        </ul>
    </div>
</template>
  
<script>
import { BadgeCheckIcon, TrashIcon, FolderAddIcon   } from "@vue-hero-icons/outline"
export default {
    name: "todo-item",
    data() {
        return {
            icons: [BadgeCheckIcon, FolderAddIcon, TrashIcon ],
            newTask: '',
            newDate: '',
            today: {},
            tasks: []
        };
    },

    computed: {
        pendingTasks() {
            return this.tasks.filter(task => !task.completed);
        },
        completedTasks() {
            return this.tasks.filter(task => task.completed);
        },
    },
    methods: {
        addTask() {
            if (this.newTask.trim() !== '' && this.newDate.trim() !== '') {
                this.tasks.push({
                    id: Date.now(),
                    description: this.newTask,
                    completed: false,
                    read: false,
                    workDate: this.newDate
                });
                this.newTask = '';
                this.newDate = '';
            }
        },
        readTask(task) { task.read = true; },
        completeTask(task) { task.completed = true; },
        duplicateTask(task) {
            this.tasks.push({
                id: Date.now(),
                description: task.description,
                completed: false,
                read: false,
                workDate: task.workDate
            });
        },
        deleteTask(task) {
            const index = this.tasks.indexOf(task);
            if (index > -1) this.tasks.splice(index, 1);

        },

        taskStyle(task) {
            const today = new Date();
            const currentDate = new Date(today.getFullYear(), today.getMonth(), today.getDate());
            const workDate = new Date(task.workDate);
            if (workDate < currentDate) {     return { color: 'red' };       }
            return { color: 'green' };
        }
    }
};
</script>
  
<style>/* Add your Tailwind CSS classes here */</style>