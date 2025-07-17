<template>
  <div class="detalle-modal">
    <button class="cerrar" @click="$emit('cerrar')">✖</button>

    <img
      :src="require(`@/assets/${producto.imagen}`)"
      alt="producto"
      class="producto-img"
    />

    <div class="info">
      <h2>{{ producto.nombre }}</h2>
      <p><strong>SKU:</strong> {{ producto.sku }}</p>
      <p class="descripcion">{{ producto.descripcion }}</p>

      <p>
        <strong>Precio actual:</strong>
        <span class="precio">${{ producto.precio }}</span>
      </p>

      <div class="cantidad-box">
        <label for="cantidad">Cantidad:</label>
        <input id="cantidad" type="number" v-model.number="cantidad" min="1" />
      </div>

      <p class="total">
        <strong>Total:</strong> ${{ producto.precio * cantidad }}
      </p>

      <button class="whatsapp-btn" @click="agregarAlCarrito">
        🛒 Agregar al carrito
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["producto"],
  data() {
    return {
      cantidad: 1,
    };
  },
  methods: {
    agregarAlCarrito() {
      this.$emit("agregar", this.producto, this.cantidad);
    },
  },
};
</script>

<style scoped>
.detalle-modal {
  position: relative;
  padding: 20px;
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  text-align: left;
  color: #333;
  max-width: 500px;
  margin: auto;
}

.cerrar {
  position: absolute;
  top: 10px;
  right: 15px;
  background: transparent;
  border: none;
  font-size: 22px;
  cursor: pointer;
  color: #999;
}

.producto-img {
  width: 100%;
  border-radius: 10px;
  margin-bottom: 15px;
  object-fit: cover;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.info h2 {
  font-size: 24px;
  margin-bottom: 10px;
  color: #222;
}

.descripcion {
  font-style: italic;
  color: #666;
  margin-bottom: 10px;
}

.precio {
  color: #e91e63;
  font-weight: bold;
}

.cantidad-box {
  margin-top: 15px;
}

.cantidad-box input {
  width: 60px;
  padding: 5px;
  margin-left: 10px;
}

.total {
  margin-top: 10px;
  font-size: 18px;
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
  transition: background-color 0.3s ease;
  cursor: pointer;
}

.whatsapp-btn:hover {
  background-color: #1ebe5d;
}
</style>
