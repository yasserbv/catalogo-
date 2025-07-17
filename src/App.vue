<template>
  <div id="app">
    <div class="banner">
      <h1>🛍️ Catálogo Sex Shop</h1>
    </div>

    <ProductList @ver-detalle="productoSeleccionado = $event" />

    <!-- Modal de producto -->
    <div
      v-if="productoSeleccionado"
      class="modal-fondo"
      @click.self="cerrarModal"
    >
      <div class="modal-contenido">
        <ProductDetail
          :producto="productoSeleccionado"
          @cerrar="cerrarModal"
          @agregar="agregarAlCarrito"
        />
      </div>
    </div>

    <!-- Modal del carrito -->
    <div
      v-if="mostrarCarrito"
      class="modal-fondo"
      @click.self="mostrarCarrito = false"
    >
      <div class="modal-contenido">
        <h2>🛒 Tu carrito</h2>
        <ul v-if="carrito.length">
          <li v-for="(item, i) in carrito" :key="i" class="carrito-item">
            <span>
              {{ item.nombre }} x{{ item.cantidad }} - ${{
                item.precio * item.cantidad
              }}
            </span>
            <button class="eliminar-btn" @click="eliminarDelCarrito(i)">
              ❌
            </button>
          </li>
        </ul>

        <p v-else>El carrito está vacío.</p>
        <p v-if="carrito.length"><strong>Total:</strong> ${{ totalCarrito }}</p>
        <a
          v-if="carrito.length"
          :href="whatsappCarrito"
          target="_blank"
          class="whatsapp-btn"
          >Enviar pedido por WhatsApp</a
        >
      </div>
    </div>

    <!-- Botón flotante del carrito -->
    <button class="carrito-float" @click="mostrarCarrito = true">
      🛒 {{ carrito.length }}
    </button>
  </div>
</template>

<script>
import ProductList from "./components/ProductList.vue";
import ProductDetail from "./components/ProductDetail.vue";

export default {
  components: { ProductList, ProductDetail },
  data() {
    return {
      productoSeleccionado: null,
      carrito: [],
      mostrarCarrito: false,
    };
  },
  computed: {
    totalCarrito() {
      return this.carrito.reduce(
        (total, item) => total + item.precio * item.cantidad,
        0
      );
    },
    whatsappCarrito() {
      let mensaje = "Hola, quiero hacer un pedido desde la tienda:\n\n";
      this.carrito.forEach((item, i) => {
        mensaje += `${i + 1}. ${item.nombre} x${item.cantidad} - $${
          item.precio * item.cantidad
        }\n`;
      });
      mensaje += `\nTotal: $${this.totalCarrito}`;
      return `https://wa.me/573103487741?text=${encodeURIComponent(mensaje)}`;
    },
  },
  methods: {
    cerrarModal() {
      this.productoSeleccionado = null;
    },
    agregarAlCarrito(producto, cantidad) {
      const index = this.carrito.findIndex((p) => p.id === producto.id);
      if (index >= 0) {
        this.carrito[index].cantidad += cantidad;
      } else {
        this.carrito.push({ ...producto, cantidad });
      }
      this.cerrarModal();
    },
    eliminarDelCarrito(index) {
      this.carrito.splice(index, 1);
    },
  },
};
</script>

<style>
body {
  margin: 0;
  font-family: "Segoe UI", sans-serif;
}

#app {
  padding: 20px;
  max-width: 900px;
  margin: auto;
}

.banner {
  background: rgba(153, 1, 156, 0.835);
  padding: 20px 30px;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  margin: 20px auto;
  text-align: center;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  max-width: 600px;
}

.banner h1 {
  margin: 0;
  color: #ffffff;
  font-size: 28px;
  font-weight: 700;
  text-shadow: 1px 1px 5px rgba(0, 0, 0, 0.5);
}

.modal-fondo {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-contenido {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  max-width: 500px;
  width: 90%;
  max-height: 90%;
  overflow-y: auto;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  color: #222;
}

.carrito-float {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #e91e63;
  color: white;
  padding: 12px 18px;
  border-radius: 50px;
  border: none;
  font-size: 18px;
  cursor: pointer;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  z-index: 1001;
}

.whatsapp-btn {
  display: inline-block;
  margin-top: 20px;
  background-color: #25d366;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  text-decoration: none;
  font-weight: bold;
  font-size: 16px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}

.carrito-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
  background: #f8f8f8;
  padding: 8px 12px;
  border-radius: 6px;
  color: #333;
}

.eliminar-btn {
  background: transparent;
  border: none;
  color: #e53935;
  font-size: 18px;
  cursor: pointer;
}
.eliminar-btn:hover {
  color: #b71c1c;
}
</style>
