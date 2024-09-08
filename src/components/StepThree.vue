<template>
  <div class="step-three">
    <h2>Doświadczenie zawodowe</h2>
    <table class="experience-table">
      <tr class="single-company" v-for="(exp, index) in data" :key="index">
        <td>
          <input 
            type="text" 
            v-model="exp.company" 
            placeholder="Firma" 
            class="form-input" 
            :class="{'input-error': errors[index]?.company}" 
          />
          <span v-if="errors[index]?.company" class="error-msg">Nazwa firmy jest wymagana</span>
        </td>
        <td>
          <input 
            type="text" 
            v-model="exp.position" 
            placeholder="Stanowisko" 
            class="form-input" 
            :class="{'input-error': errors[index]?.position}"
          />
          <span v-if="errors[index]?.position" class="error-msg">Stanowisko jest wymagane</span>
        </td>
        <td>
          <input 
            type="date" 
            v-model="exp.startDate" 
            class="form-input" 
            :class="{'input-error': errors[index]?.startDate}" 
          />
          <span v-if="errors[index]?.startDate" class="error-msg">Nieprawidłowa data początkowa</span>
        </td>
        <td>
          <input 
            type="date" 
            placeholder="Koniec"
            v-model="exp.endDate" 
            :min="exp.startDate" 
            class="form-input" 
            :class="{'input-error': errors[index]?.endDate}" 
          />
          <span v-if="errors[index]?.endDate" class="error-msg">Nieprawidłowa data końcowa</span>
        </td>
        <td><button @click="removeExperience(index)" class="remove-btn">Usuń</button></td>
      </tr>
    </table>
    <button class="add-btn" @click="addExperience">Dodaj doświadczenie</button>
    <div class="navigation-buttons">
      <button class="prev-btn" @click="previousStep">Poprzedni</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['modelValue'],
  data() {
    return {
      data: this.modelValue,
      errors: []
    };
  },
  methods: {
    validate() {
      this.errors = [];
      this.data.forEach((exp, index) => {
        this.errors[index] = {
          company: !exp.company,
          position: !exp.position,
          startDate: !exp.startDate,
          endDate: !exp.endDate || exp.endDate < exp.startDate
        };
      });

      return this.errors.every(error => 
        !error.company && !error.position && !error.startDate && !error.endDate
      );
    },
    previousStep() {
      this.$emit('previous');
    },
    addExperience() {
      this.data.push({ company: '', position: '', startDate: '', endDate: '' });
    },
    removeExperience(index) {
      this.data.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.step-three {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

h2 {
  margin-bottom: 20px;
  color: #333;
  text-align: center;
}

.single-company {
  height: 40px;
}

.experience-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

.experience-table td {
  padding: 2px;
  vertical-align: top;
}

.form-input {
  width: 100%;
  height: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  margin-bottom: 5px;
}

.input-error {
  border-color: red;
}

.error-msg {
  color: red;
  font-size: 12px;
  margin-top: -5px;
}

.add-btn {
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  margin-bottom: 20px;
}

.add-btn:hover {
  background-color: #45a049;
}

.remove-btn {
  margin-block: auto;
  background-color: #f44336;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px;
  cursor: pointer;
}

.remove-btn:hover {
  background-color: #d32f2f;
}

.navigation-buttons {
  display: flex;
  justify-content: space-between;
}

.prev-btn {
  padding: 10px;
  background-color: #008CBA;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.prev-btn:hover {
  background-color: #007B9A;
}
</style>
