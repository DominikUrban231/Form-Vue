<template>
  <div class="step-two">
    <h2>Dane kontaktowe</h2>
    <input 
      type="text" 
      v-model="data.phone" 
      placeholder="Telefon" 
      class="form-input" 
      :class="{'input-error': errors.phone}"
    />
    <span v-if="errors.phone" class="error-msg">Nieprawidłowy numer telefonu</span>

    <input 
      type="email" 
      v-model="data.email" 
      placeholder="E-mail" 
      class="form-input" 
      :class="{'input-error': errors.email}"
    />
    <span v-if="errors.email" class="error-msg">Nieprawidłowy adres e-mail</span>

    <div class="navigation-buttons">
      <button class="prev-btn" @click="previousStep">Poprzedni krok</button>
      <button class="next-btn" @click="nextStep">Następny krok</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['modelValue'],
  data() {
    return {
      data: this.modelValue,
      errors: {
        phone: false,
        email: false
      }
    };
  },
  methods: {
    validate() {
      const phoneRegex = /^\d{9,15}$/;
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

      this.errors.phone = !phoneRegex.test(this.data.phone);
      this.errors.email = !emailRegex.test(this.data.email);

      return !this.errors.phone && !this.errors.email;
    },
    nextStep() {
      if (this.validate()) {
        this.$emit('next', this.data);
      }
    },
    previousStep() {
      this.$emit('previous');
    }
  }
};
</script>

<style scoped>
.step-two {
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

.navigation-buttons {
  display: flex;
  justify-content: space-between;
}

.next-btn, .prev-btn {
  padding: 10px;
  background-color: #008CBA;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.next-btn:hover, .prev-btn:hover {
  background-color: #007B9A;
}
</style>
