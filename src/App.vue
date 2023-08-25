<script setup>
import { ref } from 'vue';

import img1 from "/src/assets/ecommerce1.jpg"
import img2 from "/src/assets/ecommerce2.jpg"
import img3 from "/src/assets/ecommerce3.jpg"
import img4 from "/src/assets/ecommerce4.jpg"
import img5 from "/src/assets/ecommerce5.jpg"
import img6 from "/src/assets/ecommerce6.jpg"
import img7 from "/src/assets/ecommerce7.jpg"
import img8 from "/src/assets/ecommerce8.jpg"
import img9 from "/src/assets/ecommerce9.jpg"
import img10 from "/src/assets/ecommerce10.jpg"
import img11 from "/src/assets/ecommerce11.jpg"
import img12 from "/src/assets/ecommerce12.jpg"


let info = ref([
  { img: img1, nombre: "Bikini 1", precio_antes: "$50.000", precio_ahora: "$35.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img2, nombre: "Bikini 2", precio_antes: "$60.000", precio_ahora: "$40.000", talla: "M", material: "Seda 80% - Algodon 20%" },
  { img: img3, nombre: "Bikini 3", precio_antes: "$70.000", precio_ahora: "$38.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img4, nombre: "Bikini 4", precio_antes: "$50.000", precio_ahora: "$33.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img5, nombre: "Bikini 5", precio_antes: "$50.000", precio_ahora: "$37.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img6, nombre: "Bikini 6", precio_antes: "$41.000", precio_ahora: "$28.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img7, nombre: "Bikini 7", precio_antes: "$70.000", precio_ahora: "$55.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img8, nombre: "Bikini 8", precio_antes: "$50.000", precio_ahora: "$45.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img9, nombre: "Bikini 9", precio_antes: "$50.000", precio_ahora: "$42.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img10, nombre: "Bikini 10", precio_antes: "$50.000", precio_ahora: "$31.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img11, nombre: "Bikini 11", precio_antes: "$50.000", precio_ahora: "$29.000", talla: "S", material: "Seda 80% - Algodon 20%" },
  { img: img12, nombre: "Bikini 12", precio_antes: "$80.000", precio_ahora: "$49.000", talla: "S", material: "Seda 80% - Algodon 20%" }
])

function eliminar(index) {
  carrito.value.splice(index, 1);
}

let carrito = ref([]);

let mostrarCarrito = ref(false);

function toggleMostrarCarrito() {
  mostrarCarrito.value = !mostrarCarrito.value;
}

function agregarAlCarrito(producto) {
  const carritoIndex = carrito.value.findIndex(item => item.nombre === producto.nombre);

  if (carritoIndex === -1) {
    carrito.value.push({ ...producto, cantidad: 1 });
  } else {
    carrito.value[carritoIndex].cantidad++;
  }
}

function aumentarCantidad(index) {
  carrito.value[index].cantidad++;
}

function disminuirCantidad(index) {
  if (carrito.value[index].cantidad > 1) {
    carrito.value[index].cantidad--;
  }
}

function calcularTotalGlobal() {
  let total = 0;
  carrito.value.forEach(item => {
    total += item.cantidad * parseFloat(item.precio_ahora.replace('$', '').replace(',', ''));
  });
  const totalCOP = (total * 1000).toLocaleString("es-CO", {
    style: "currency",
    currency: "COP"
  });
  return totalCOP;
}

function vaciarCarrito() {
  carrito.value = [];
}

</script>
<template>
  <div class="main">
    <div class="encabezado">
      <h1>Euforia Boutique</h1>
      <div class="carrito-icon" @click="toggleMostrarCarrito">
        <img src="/src/assets/carrito.png" alt="">
      </div>
    </div>
    <div class="container-cards">
      <div v-for="(item, i) in info" :key="i" class="card">
        <img :src="item.img" alt="Producto" class="img">

        <p>Producto: {{ item.nombre }} </p>
        <div class="precio-antes-rayita"></div>
        <p>Precio Antes: {{ item.precio_antes }}</p>
        <p class="ahora">Precio Ahora: {{ item.precio_ahora }}</p>
        <p>Talla: {{ item.talla }} </p>
        <p>Material: {{ item.material }}</p>

        <button @click="agregarAlCarrito(item)">Comprar</button>
      </div>
    </div>
    <div v-if="mostrarCarrito" class="modal-background" style="text-align: center;">
      <div class="modal">
        <h2>Carrito de Compras</h2>
        <div v-for="(item, index) in carrito" :key="index" class="carrito-item">
          <!-- ... Mostrar información del producto en el carrito ... -->
          <img :src="item.img" alt="Producto" class="img">
          <p>{{ item.nombre }}</p>
          <p>Precio: {{ item.precio_ahora }}</p>
          <p>Cantidad: {{ item.cantidad }}</p>
          <button @click="aumentarCantidad(index)">+</button>
          <button @click="disminuirCantidad(index)">-</button>
          <button @click="eliminar(index)">❌</button>
        </div>
        <p class="total-global">Total a Pagar: {{ calcularTotalGlobal() }}</p>
        <button @click="vaciarCarrito" class="vaciar-carrito" style="margin-right: 10px;">Vaciar Carrito</button>
        <button @click="toggleMostrarCarrito" class="cerrar-modal">Cerrar</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Petemoss&family=Pinyon+Script&display=swap');

.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color:#e7c5dc;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  max-width: 80%;
  max-height: 80%;
  overflow-y: auto;
  color: #000000;
}

.cerrar-modal {
  margin-top: 10px;
}
.main {
  margin: 0%;
  width: 100%;
  display: flex;
  flex-direction: column;
}

h1 {
  font-family: 'Pinyon Script', cursive;
  font-size: 100px;
  margin: 0;
  text-align: center;
  margin-left: 20px;
  color: rgb(209, 183, 32);
  font-weight: bold;
}

p {
  padding-top: 3px;
  margin: 1%;
}

.container-cards {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: 100%;
  justify-content: center;
}

.img {
  max-height: 200px;
  max-width: 200px;
  transition: filter 300ms;
  border-radius: 10px;
  padding-bottom: 20px;
}

.encabezado {
  background-color: #e9c3f3;
  margin: 0%;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
}

.carrito-icon {
  border-radius: 50px;
  max-height: 70px;
  width: 70px;
  padding: 0%;
  margin-left: 48%;
  cursor: pointer;
  margin-bottom: 5px;
  margin-top: -2%
}

.carrito-icon img {
  margin-left: 10%;
  margin-top: 12%;
  max-width: 50px;
}

.card {
  display: flex;
  align-items: center;
  background-color: aliceblue;
  border-radius: 10px;
  margin: 20px;
  color: black;
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  flex-direction: column;
  text-align: justify;
  max-width: 450px;
}

.total-global{
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}

.precio-antes-rayita {
  position: relative;
  width: 28%;
  height: 2px;
  background-color: rgba(255, 0, 0, 0.527);
  top: 20px;
  left: 20%;
}

.ahora {
  color: rgb(0, 238, 32);
  font-weight: bold;
}

.card:hover {
  filter: drop-shadow(0 0 2em #c71ce9aa);
}

button {
  max-height: 50px;
}

.carrito {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
  padding: 20px;
  border: 1px solid #000000;
  border-radius: 10px;
  background-color: #000000;
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}

.carrito-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.carrito-item img {
  max-height: 100px;
  max-width: 80px;
  margin-right: 10px;
  box-shadow: 2px 2px 2px #000000;
  padding-bottom: 0;
}

.carrito-item button {
  margin-left: 10px;
  padding: 5px 10px;
  border: none;
  cursor: pointer;
}
</style>
