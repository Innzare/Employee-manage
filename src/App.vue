<template>
  <div class="app-wrapper">
    <Header :activePage="activePage" @onChangePage="(page) => activePage = page" />

    <main class="main">
      <div class="container">
        <Form
          v-if="isFormPage"
          :employees="employeesTemplate"
          :isFormChanged="isFormChanged"
          @addEmployee="addEmployee"
          @removeEmployee="removeEmployee"
          @saveData="saveData"
          @cancelChanging="cancelChanging"
        />

        <Preview v-if="isPreviewPage" :employees="employees" />
      </div>
    </main>

    <Footer />
  </div>
</template>

<script>
import { defineComponent, ref, computed } from 'vue';
import { ACTIVE_PAGE } from '@/consts/app';
import { isObjectsEqual, getJSONParsedData } from '@/helpers/common';
import { EmployeeController } from '@/controller/EmployeeController';
import Preview from '@/views/Preview'
import Form from '@/views/Form'
import Header from '@/components/Layout/Header'
import Footer from '@/components/Layout/Footer'

export default defineComponent({
  name: 'App',
  components: {
    Preview,
    Form,
    Header,
    Footer
  },

  setup() {
    const activePage = ref(ACTIVE_PAGE.PREVIEW);
    const employeeController = new EmployeeController();

    const employees = employeeController.getEmployees();
    const employeesTemplate = ref(getJSONParsedData(employees.value));

    const addEmployee = (employee) => {
      employeesTemplate.value.push(employee);
    }

    const removeEmployee = (employee) => {
      employeesTemplate.value = employeesTemplate.value.filter(item => item.id !== employee.id);
    }

    const saveData = () => {
      employeeController.setEmployees(getJSONParsedData(employeesTemplate.value));
    }

    const cancelChanging = () => {
      employeesTemplate.value = getJSONParsedData(employees.value);
    }

    const isFormChanged = computed(() => {
      return !isObjectsEqual(employees.value, employeesTemplate.value)
    });

    const isPreviewPage = computed(() => {
      return activePage.value === ACTIVE_PAGE.PREVIEW
    });

    const isFormPage = computed(() => {
      return activePage.value === ACTIVE_PAGE.FORM
    });

    return {
      employees,
      employeesTemplate,
      activePage,

      isPreviewPage,
      isFormPage,
      isFormChanged,

      addEmployee,
      removeEmployee,
      saveData,
      cancelChanging
    };
  }
});
</script>

<style lang='scss'>
@import "@/assets/styles/main.scss";

.app-wrapper {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.main {
  flex: 1;
  padding: 30px 0;

  @media(max-width: 768px) {
    padding-inline: 15px;
  }

  .container {
    max-width: 90%;
    height: 100%;
    margin-inline: auto;

    @media(max-width: 768px) {
      max-width: 100%;
    }
  }
}
</style>