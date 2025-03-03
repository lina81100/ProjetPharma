<template>
  <div id="app">
    <div class="header">
      <img src="/Logo-Pharmacie.jpg" alt="Logo" class="logo" />
      <h1>La Pharmacie de Lina</h1>
    </div>
    <MedicineList />
  </div>
</template>

<script>
import MedicineList from "./components/ListeMedicaments.vue";

export default {
  components: {
    MedicineList,
  },
};
</script>

<style>
@import "./assets/base.css";

#app {
  background-image: url("/fond3.jpg");
  background-size: cover;
  background-position: center center;
  background-attachment: fixed;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  margin: 0;
}

body {
  margin: 0;
  padding: 0;
  height: 100%;
}

.header {
  display: flex;
  align-items: center;
  background-color: #047b1f;
  color: #fff;
  padding: 20px 40px;
  border-radius: 8px;
  margin-bottom: 0px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 700px;
  text-align: center;
}

.logo {
  width: 50px;
  height: 50px;
  margin-right: 20px;
  border-radius: 50%;
}

h1 {
  margin: 0;
  font-size: 2em;
  font-weight: bold;
}
</style>
