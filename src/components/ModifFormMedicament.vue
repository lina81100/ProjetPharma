<template>
  <form @submit.prevent="mettreAJourMedicament" class="modification-form">
    <input v-model="medicament.denomination" placeholder="Nom" />
    <input v-model="medicament.formepharmaceutique" placeholder="Forme" />
    <input v-model.number="medicament.qte" type="number" placeholder="Quantité" />
    <input type="file" @change="handleFileUpload" />
    <button type="submit" class="button">Valider</button>
    <button type="button" @click="annulerModification" class="button">Annuler</button>
  </form>
</template>

<script>
import { ref } from "vue";

export default {
  name: "MedicineEditForm",
  props: {
    medicament: Object,
  },
  setup(props, { emit }) {
    const photoModification = ref(null);

    const mettreAJourMedicament = () => {
      const payload = {
        id: props.medicament.id,
        denomination: props.medicament.denomination,
        formepharmaceutique: props.medicament.formepharmaceutique,
        qte: props.medicament.qte,
      };

      if (photoModification.value) {
        payload.photo = photoModification.value;
      }

      emit("medicament-modifie", payload);
    };

    const annulerModification = () => {
      emit("medicament-modifie");
    };

    const handleFileUpload = (e) => {
      const file = e.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = () => {
        photoModification.value = reader.result;
      };
      reader.readAsDataURL(file);
    };

    return {
      mettreAJourMedicament,
      annulerModification,
      handleFileUpload,
    };
  },
};
</script>

<style scoped>
.modification-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.modification-form input {
  padding: 12px;
  font-size: 16px;
  border-radius: 8px;
  border: 1px solid #ccc;
  outline: none;
}

.modification-form input:focus {
  border-color: #f0ad4e;
}

.modification-form .button {
  align-self: flex-start;
  padding: 12px 20px;
  background-color: #f0ad4e;
  color: #fff;
  border-radius: 8px;
  font-size: 16px;
  transition: background-color 0.3s, transform 0.3s;
}

.modification-form .button:hover {
  background-color: #ec971f;
  transform: scale(1.05);
}

.modification-form .button:active {
  background-color: #d9534f;
}
</style>
