<template>
  <div class="preview">
    <div class="preview__employees">
      <h1 class="preview__title">Сотрудники</h1>

      <div class="preview__employee-wrapper" v-if="isEmployeesExist">
        <div class="preview__employee" v-for="(employee, index) of employeesInfo">
          <div># {{ index + 1 }}</div>

          <div class="preview__employee-info-wrapper">
            <span>Имя: </span>
            <span class="preview__employee-info">{{ employee.name }}</span>
          </div>

          <div class="preview__employee-info-wrapper">
            <span>Фамилия: </span>
            <span class="preview__employee-info">{{ employee.surname }}</span>
          </div>

          <div class="preview__employee-info-wrapper">
            <span>Возраст:</span>
            <span class="preview__employee-info">{{ employee.age }}</span>
          </div>

          <div class="preview__employee-info-wrapper">
            <span>Стаж:</span>
            <span class="preview__employee-info">{{ employee.experience }}</span>
          </div>

          <div class="preview__employee-info-wrapper">
            <span>Адрес:</span>
            <span class="preview__employee-info">{{ employee.address }}</span>
          </div>
        </div>
      </div>

      <div v-else class="preview__no-employees">Сотрудники не указаны</div>
    </div>
  </div>
</template>

<script>
import { defineComponent, computed } from 'vue';
import { getAgeFormatted } from '@/helpers/common';

export default defineComponent({
  name: 'Preview',

  props: {
    employees: {
      type: Array,
      default: []
    }
  },

  setup(props) {
    const { employees } = props;

    const isEmployeesExist = computed(() => employees.length > 0);

    const employeesInfo = computed(() => {
      return employees.map(item => {
        const { name, surname, age, experience, address } = item;

        const ageFormatted = getAgeFormatted(age);
        const experienceFormatted = getAgeFormatted(age);
        const employeeName = name !== '' ? name : 'Имя не указано';
        const employeeSurname = surname !== '' ? surname : 'Фамилия не указана';
        const employeeAge = age !== '' ? ageFormatted : 'Возраст не указан';
        const employeeExperience = experience !== '' ? experienceFormatted : 'Стаж не указан';
        const employeeAddress = address !== '' ? address : 'Адрес не указан';

        return {
          name: employeeName,
          surname: employeeSurname,
          age: employeeAge,
          experience: employeeExperience,
          address: employeeAddress
        };
      })
    });

    return { isEmployeesExist, employeesInfo }
  }
})
</script>

<style lang="scss" scoped>
  .preview {
    animation-name: fade-in;
    animation-duration: 0.5s;
    animation-fill-mode: forwards;

    &__title {
      font-weight: 500;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
    }

    &__personal-data-wrapper {
      display: flex;
      flex-direction: column;
      gap: 20px;
      margin-bottom: 60px;
    }

    &__personal-data {
      font-style: normal;
      font-weight: 700;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
    }

    &__employees {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 20px;
    }

    &__no-employees {
      font-style: normal;
      font-weight: 700;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
    }

    &__employee-wrapper {
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      gap: 16px;
    }

    &__employee {
      flex: 0 0 32%;
      display: flex;
      flex-direction: column;
      padding: 10px 20px;
      background: #F1F1F1;
      border-radius: 5px;
      font-style: normal;
      font-weight: 700;
      font-size: 16px;
      line-height: 24px;
      color: #111111;

      @media(max-width: 1200px) {
        flex: 0 0 48%;
      }

      @media(max-width: 768px) {
        flex: 0 0 100%;
      }

      .preview__employee-info-wrapper {
        display: flex;
        justify-content: space-between;
        gap: 16px;

        .preview__employee-info {
          font-weight: normal;
        }
      }
    }
  }
</style>