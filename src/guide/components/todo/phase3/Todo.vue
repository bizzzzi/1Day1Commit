<script setup lang="ts">
import type { ITodo } from "~/guide/types";
import { TODO_STATUS } from "~/guide/types/enums";
import { useTodoStore } from "~/guide/store/todo";

const props = defineProps<{ todo: ITodo }>();
const todoStore = useTodoStore();
const { doCheck, doUncheck, changeTodoStatus } = todoStore;

const todo = $ref<ITodo>(Object.assign({}, props.todo));
const check = $ref(false);

const changeStatus = () => {
  if (todo.status === TODO_STATUS.DONE || todo.status === TODO_STATUS.EXCEPTED)
    todo.done = true;
  else todo.done = false;

  changeTodoStatus(todo);
};

const changeCheck = () => {
  if (check) doCheck(todo);
  else doUncheck(todo);
};
</script>

<template>
  <tr>
    <td class="border border-slate-300">
      <input v-model="check" type="checkbox" @change="changeCheck" />
    </td>
    <td class="border border-slate-300">
      <select
        v-model="todo.status"
        class="border my-1 w-10/12"
        @change="changeStatus"
      >
        <option :value="TODO_STATUS.PREPARE">준비</option>
        <option :value="TODO_STATUS.IN_PROGRESS">진행 중</option>
        <option :value="TODO_STATUS.DONE">완료</option>
        <option :value="TODO_STATUS.EXCEPTED">제외</option>
      </select>
    </td>
    <!-- text -->
    <td class="border border-slate-300">
      <p
        :class="`${todo.done ? 'line-through text-gray-400' : ''} ${
          todo.status === TODO_STATUS.IN_PROGRESS
            ? 'font-bold text-green-600'
            : ''
        }`"
      >
        {{ todo.text }}
      </p>
    </td>
  </tr>
</template>
