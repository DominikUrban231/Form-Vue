<template>
  <div class="step-one">
    <h2>Dane podstawowe</h2>
    <input 
      type="text" 
      v-model="data.firstName" 
      placeholder="Imię" 
      class="form-input" 
      :class="{'input-error': errors.firstName}"
    />
    <span v-if="errors.firstName" class="error-msg">Imię jest wymagane</span>

    <input 
      type="text" 
      v-model="data.lastName" 
      placeholder="Nazwisko" 
      class="form-input" 
      :class="{'input-error': errors.lastName}"
    />
    <span v-if="errors.lastName" class="error-msg">Nazwisko jest wymagane</span>

    <input 
      type="date" 
      v-model="data.birthDate" 
      :min="today" 
      class="form-input" 
      :class="{'input-error': errors.birthDate}"
    />
    <span v-if="errors.birthDate" class="error-msg">Data urodzenia jest wymagana i musi być późniejsza niż dzisiejsza data</span>

    <button class="next-btn" @click="nextStep">Następny krok</button>
  </div>
</template>

<script>
export default {
  props: ['modelValue'],
  data() {
    return {
      data: this.modelValue,
      today: new Date().toISOString().split('T')[0],
      errors: {
        firstName: false,
        lastName: false,
        birthDate: false
      }
    };
  },
  methods: {
    validate() {
      this.errors.firstName = !this.data.firstName;
      this.errors.lastName = !this.data.lastName;
      this.errors.birthDate = !this.data.birthDate || this.data.birthDate <= this.today;

      return !this.errors.firstName && !this.errors.lastName && !this.errors.birthDate;
    },
    nextStep() {
      if (this.validate()) {
        this.$emit('next', this.data);
      }
    }
  }
};
</script>

<style scoped>
.step-one {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

h2 {
  margin-bottom: 20px;
  color: #333;
}

.form-input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.input-error {
  border-color: red;
}

.error-msg {
  color: red;
  font-size: 12px;
}

.next-btn {
  padding: 10px;
  background-color: #008CBA;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.next-btn:hover {
  background-color: #007B9A;
}
</style>
