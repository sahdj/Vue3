<!--
 * @Author: your name
 * @Date: 2021-04-22 15:27:37
 * @LastEditTime: 2021-04-25 14:56:34
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \Vue3test\vue3-todo\src\components\TodoList\Item.vue
-->
<template>
  <div>
    <input 
      type="checkbox"
      :checked="item.status === 'FINISHED'"
      @click="setStatus(item.id)"
    />
    <span 
      :class="item.status === FINISHED ? 'line-through' : ''"
    >
      {{ item.content }}
    </span>
    <button
      @click="removeTodo(item.id)"
    >删除</button>
    <button
      v-if="item.status !== FINISHED"
      @click="setDoing(item.id)"
      :class="item.status === DOING ? 'doing' : 'willdo'"
    >
      {{ item.status === DOING ? '正在做...' : '马上做' }}
    </button>
  </div>
</template>

<script lang="ts">
import { ITodo, TODO_STATUS } from '@/typings';
import { defineComponent, PropType } from 'vue';

interface IStatusState {
  DOING: TODO_STATUS;
  FINISHED: TODO_STATUS;
  WILLDO: TODO_STATUS;
}

export default defineComponent ({
  name:'TodoItem',
  props: {
    item: Object as PropType<ITodo>
  },
  setup(props, { emit }) {
    const statusState: IStatusState = {
      DOING: TODO_STATUS.DOING,
      FINISHED: TODO_STATUS.FINISHED,
      WILLDO: TODO_STATUS.WILLDO
    }

    const removeTodo = (id: number): void => {
      emit('removeTodo', id);
    }

    const setStatus = (id: number): void => {
      emit('setStatus', id);
    }

    const setDoing = (id: number): void => {
      emit('setDoing', id);
    }

    return {
      ...statusState,
      removeTodo,
      setStatus,
      setDoing
    }
  }
})
</script>

<style> 
  .line-through {
    text-decoration: line-through;
  }

  .doing {
    background-color: pink;
    color: orange;
  }

  .willdo {
    background-color: skyblue;
    color: red;
  }
</style>