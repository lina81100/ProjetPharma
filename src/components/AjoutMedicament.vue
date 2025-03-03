<template>
  <form @submit.prevent="ajouterMedicament" class="add-form">
    <input v-model="nouveauMedicament.denomination" placeholder="Dénomination" required />
    <input v-model="nouveauMedicament.formepharmaceutique" placeholder="Forme pharmaceutique" required />
    <input v-model.number="nouveauMedicament.qte" type="number" placeholder="Quantité" required />
    <input type="file" @change="handleFileUpload" />
    <button type="submit" class="button">Ajouter</button>
  </form>
</template>

<script>
import { ref } from "vue";

export default {
  name: "AjoutMedicament",
  setup(_, { emit }) {
    const nouveauMedicament = ref({
      denomination: "",
      formepharmaceutique: "",
      qte: 1,
      photo: null,
    });

    const ajouterMedicament = () => {
      emit("medicament-ajoute", { ...nouveauMedicament.value });
      nouveauMedicament.value = { denomination: "", formepharmaceutique: "", qte: 1, photo: null };
    };

    const handleFileUpload = (e) => {
      const file = e.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = () => {
        nouveauMedicament.value.photo = reader.result;
      };
      reader.readAsDataURL(file);
    };

    return {
      nouveauMedicament,
      ajouterMedicament,
      handleFileUpload,
    };
  },
};
</script>

<style scoped>
.add-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.add-form input {
  padding: 12px;
  font-size: 16px;
  border-radius: 8px;
  border: 1px solid #ccc;
  outline: none;
}

.add-form input:focus {
  border-color: #5cb85c;
}

.add-form .button {
  align-self: flex-start;
  padding: 12px 20px;
  background-color: #5cb85c;
  color: #fff;
  border-radius: 8px;
  font-size: 16px;
  transition: background-color 0.3s, transform 0.3s;
}

.add-form .button:hover {
  background-color: #4cae4c;
  transform: scale(1.05);
}

.add-form .button:active {
  background-color: #3e8e3e;
}
</style>
