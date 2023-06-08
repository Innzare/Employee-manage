<template>
  <div class="form">
    <EmployeeList v-model:employees="employees" @addEmployee="addEmployee" @removeEmployee="removeEmployee" />

    <div class="save-button-wrapper">
      <ActionButton text="Сохранить" type="filled" @click="$emit('saveData')" :disabled="!isFormChanged" />
      <ActionButton v-if="isFormChanged" text="Отменить" @click="$emit('cancelChanging')" />
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import EmployeeList from '@/components/Employee/EmployeeList';
import ActionButton from '@/components/ActionButton';

export default defineComponent({
  name: 'Form',
  components: {
    EmployeeList,
    ActionButton
  },

  props: {
    employees: {
      type: Array,
      default: []
    },
    isFormChanged: Boolean
  },

  setup(props, { emit }) {
    const addEmployee = (employee) => {
      emit('addEmployee', employee);
    };

    const removeEmployee = (employee) => {
      emit('removeEmployee', employee);
    };

    return { addEmployee, removeEmployee };
  }
})
</script>

<style lang="scss" scoped>
  .form {
    display: flex;
    flex-direction: column;
    gap: 33px;
    height: 100%;
    animation-name: fade-in;
    animation-duration: 0.5s;
    animation-fill-mode: forwards;

    .save-button-wrapper {
      display: flex;
      justify-content: flex-start;
      gap: 10px;
    }
  }
</style>