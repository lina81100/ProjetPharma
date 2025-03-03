<template>
  <div class="medicament-card">
    <img :src="obtenirUrlPhoto(medicament.photo)" alt="Photo médicament" width="50" />
    <div class="medicament-info">
      {{ medicament.denomination }} - {{ medicament.formepharmaceutique }} - Qte: {{ medicament.qte }}
    </div>
    <div class="medicament-actions">
      <button @click="supprimerMedicament" class="button">Supprimer</button>
      <button @click="ouvrirModification" class="button">Modifier</button>
      <button @click="incrementerQuantite" class="button">+1</button>
      <button @click="decrementerQuantite" class="button">-1</button>
    </div>

    <MedicineEditForm
      v-if="medicamentSelectionne"
      :medicament="medicamentSelectionne"
      @medicament-modifie="fermerModification"
    />
  </div>
</template>

<script>
import { ref } from "vue";
import MedicineEditForm from "./ModifFormMedicament.vue";

export default {
  name: "MedicineCard",
  components: {
    MedicineEditForm,
  },
  props: {
    medicament: Object,
  },
  setup(props, { emit }) {
    const medicamentSelectionne = ref(null);

    const supprimerMedicament = () => {
      emit("medicament-supprime", props.medicament.id);
    };

    const ouvrirModification = () => {
      medicamentSelectionne.value = { ...props.medicament };
    };

    const fermerModification = (medicamentModifie) => {
      if (medicamentModifie) {
        emit("medicament-modifie", medicamentModifie);
      }
      medicamentSelectionne.value = null;
    };

    const incrementerQuantite = () => {
      emit("incrementer-quantite", props.medicament);
    };

    const decrementerQuantite = () => {
      emit("decrementer-quantite", props.medicament);
    };

    const obtenirUrlPhoto = (cheminPhoto) =>
      cheminPhoto ? `https://apipharmacie.pecatte.fr/images/${cheminPhoto}` : "";

    return {
      medicamentSelectionne,
      supprimerMedicament,
      ouvrirModification,
      fermerModification,
      incrementerQuantite,
      decrementerQuantite,
      obtenirUrlPhoto,
    };
  },
};
</script>

<style scoped>
.medicament-card {
  background-color: #f9f9f9;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.medicament-info {
  flex: 1;
  font-size: 14px;
  color: #333;
}

.medicament-actions {
  display: flex;
  gap: 10px;
}

.medicament-actions .button {
  padding: 8px 15px;
  background-color: #f0ad4e;
  color: #fff;
  font-size: 14px;
  border-radius: 5px;
  transition: background-color 0.3s, transform 0.3s;
}

.medicament-actions .button:hover {
  background-color: #ec971f;
  transform: scale(1.05);
}

.medicament-actions .button:active {
  background-color: #d9534f;
}
</style>