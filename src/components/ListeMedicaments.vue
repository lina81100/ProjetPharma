<template>
  <div class="container">
    <input
      v-model="searchQuery"
      placeholder="Rechercher un médicament..."
      class="search-bar"
    />

    <h3>Ajouter un Médicament</h3>
    <AjoutMedicament @medicament-ajoute="ajouterMedicament" />

    <div class="medicament-list">
      <CardMedicament
        v-for="medicament in medicamentsFiltrés"
        :key="medicament.id"
        :medicament="medicament"
        @medicament-supprime="supprimerMedicament"
        @medicament-modifie="mettreAJourMedicament"
        @incrementer-quantite="incrementerQuantite"
        @decrementer-quantite="decrementerQuantite"
      />
    </div>
  </div>
</template>

<script>
import { ref, onMounted, computed } from "vue";
import AjoutMedicament from "./AjoutMedicament.vue";
import CardMedicament from "./CardMedicament.vue";

export default {
  name: "ListeMedicament",
  components: {
    AjoutMedicament,
    CardMedicament,
  },
  setup() {
    const urlApi = "https://apipharmacie.pecatte.fr/api/21/medicaments";
    const medicaments = ref([]);
    const searchQuery = ref("");

    const recupererMedicaments = async () => {
      try {
        const response = await fetch(urlApi);
        if (!response.ok) {
          throw new Error("Erreur de réseau ou API indisponible");
        }
        const data = await response.json();
        medicaments.value = data.map((med) => ({
          id: med.id,
          denomination: med.denomination,
          formepharmaceutique: med.formepharmaceutique,
          qte: med.qte,
          photo: med.photo,
        }));
      } catch (error) {
        console.error("Erreur de chargement :", error);
      }
    };

    const ajouterMedicament = async (medicament) => {
      const existeDeja = medicaments.value.some(
        (med) => med.denomination.toLowerCase() === medicament.denomination.toLowerCase()
      );

      if (existeDeja) {
        alert("Un médicament avec le même nom existe déjà dans la pharmacie.");
        return;

      }

      try {
        const response = await fetch(urlApi, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(medicament),
        });
        if (response.ok) {
          recupererMedicaments();
        } else {
          const result = await response.text();
          console.error("Erreur lors de l'ajout :", result);
          alert("Erreur lors de l'ajout du médicament. Veuillez vérifier les données.");
        }
      } catch (error) {
        console.error("Erreur lors de l'ajout :", error);
      }
    };

    const supprimerMedicament = async (id) => {
      try {
        await fetch(`${urlApi}/${id}`, { method: "DELETE" });
        recupererMedicaments();
      } catch (error) {
        console.error("Erreur suppression :", error);
      }
    };

    const mettreAJourMedicament = async (medicament) => {
      try {
        const response = await fetch(urlApi, {
          method: "PUT",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            id: medicament.id,
            denomination: medicament.denomination,
            formepharmaceutique: medicament.formepharmaceutique,
            qte: medicament.qte,
            photo: medicament.photo,
          }),
        });
        if (response.ok) {
          recupererMedicaments();
        } else {
          const result = await response.json();
          console.error("Erreur lors de la modification :", result);
        }
      } catch (error) {
        console.error("Erreur lors de la modification :", error);
      }
    };

    const incrementerQuantite = async (medicament) => {
      await modifierQuantite(medicament, 1);
    };

    const decrementerQuantite = async (medicament) => {
      await modifierQuantite(medicament, -1);
    };

    const modifierQuantite = async (medicament, delta) => {
      const nouvelleQte = medicament.qte + delta;
      if (nouvelleQte < 0) return;

      try {
        const response = await fetch(urlApi, {
          method: "PUT",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            id: medicament.id,
            qte: nouvelleQte,
          }),
        });
        if (response.ok) {
          recupererMedicaments();
        } else {
          const result = await response.json();
          console.error("Erreur lors de la modification de la quantité :", result);
        }
      } catch (error) {
        console.error("Erreur lors de la modification de la quantité :", error);
      }
    };

    const medicamentsFiltrés = computed(() => {
      return medicaments.value.filter((med) =>
        med.denomination
          .toLowerCase()
          .includes(searchQuery.value.toLowerCase())
      );
    });

    onMounted(recupererMedicaments);

    return {
      medicaments,
      medicamentsFiltrés,
      searchQuery,
      ajouterMedicament,
      supprimerMedicament,
      mettreAJourMedicament,
      incrementerQuantite,
      decrementerQuantite,
    };
  },
};
</script>



<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.search-bar {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

.medicament-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
  width: 100%;
  max-width: 800px;
  margin-top: 20px;
  height: 400px;
  overflow-y: auto;
  padding-right: 10px;
}
</style>
