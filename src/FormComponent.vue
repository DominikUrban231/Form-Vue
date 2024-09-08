<template>
  <div class="form">
    <h1>Formularz rekrutacyjny</h1>
    <StepOne v-if="currentStep === 1" @next="goToStep(2)" v-model="formData.basic" />
    <StepTwo v-if="currentStep === 2" @next="goToStep(3)" @previous="goToStep(1)" v-model="formData.contact" />
    <StepThree v-if="currentStep === 3" @previous="goToStep(2)" v-model="formData.experience" />
    <button class="submit-btn" @click="submitForm" v-if="currentStep === 3">Wyślij</button>  
  </div>
</template>

<script>
import StepOne from './components/StepOne.vue'
import StepTwo from './components/StepTwo.vue'
import StepThree from './components/StepThree.vue'

export default {
  components: { 
    StepOne,
    StepTwo, 
    StepThree 
  },
  data() {
    return {
      currentStep: 1,
      formData: {
        basic: {},
        contact: {},
        experience: []
      }
    };
  },
  methods: {
    goToStep(step) {
      this.currentStep = step;
    },
    async submitForm() {
      try {
        if (!this.validateForm()) {
          alert("Proszę uzupełnić wszystkie wymagane pola.");
          return;
        }
        console.log(this.formData)

        const response = await fetch('https://example.com/api/submit', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(this.formData)
        });

        if (!response.ok) {
          throw new Error(`Wystąpił błąd przy wysyłaniu formularza\nDane formularza: ${JSON.stringify(this.formData)}`);
        }

        // Obsługa odpowiedzi serwera
        const data = await response.json();
        alert(data);

        // Resetowanie formularza po wysłaniu
        this.resetForm();
      } catch (error) {
        console.error('Błąd:', error);
        alert('Wystąpił błąd przy wysyłaniu formularza. Spróbuj ponownie później.');
      }
    },
    validateForm() {
      // Przykładowa walidacja: sprawdzanie, czy pola są wypełnione
      const { basic, contact, experience } = this.formData;
      return (
        basic.firstName && basic.lastName && basic.birthDate &&
        contact.phone && contact.email &&
        experience.length > 0 &&
        experience.every(exp => exp.company && exp.position && exp.startDate && exp.endDate)
      );
    },
    resetForm() {
      // Resetuje dane formularza
      this.formData = {
        basic: {},
        contact: {},
        experience: []
      };
      this.currentStep = 1;
    }
  }
}
</script>
<style scoped>
.form {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
}

.submit-btn {
  min-width: 100px;
  display: block;
  margin-inline: auto;
  width: 40%;
  padding: 10px;
  background-color: #d13535;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.submit-btn:hover {
  background-color: #922929;
}
</style>
