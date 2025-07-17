<template>
  <div class="grid">
    <div
      v-for="producto in productos"
      :key="producto.id"
      class="card"
      @click="$emit('ver-detalle', producto)"
    >
      <img :src="require(`@/assets/${producto.imagen}`)" alt="producto" />
      <div class="info">
        <h3>{{ producto.nombre }}</h3>
        <p><strong>SKU:</strong> {{ producto.sku }}</p>
        <p class="precio">
          <span class="precio-final">${{ producto.precio }}</span>
        </p>
        <p class="ver-mas">Ver producto</p>
      </div>
    </div>
  </div>
</template>

<script>
import productos from "@/data/products.json";

export default {
  data() {
    return { productos };
  },
};
</script>

<style scoped>
.grid {
  display: flex;
  flex-direction: column;
  gap: 25px;
  padding: 20px;
}

.card {
  display: flex;
  gap: 20px;
  align-items: center;
  background-color: #ffffff;
  padding: 15px 20px;
  border-radius: 12px;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.2s ease;
}

.card:hover {
  transform: scale(1.01);
}

.card img {
  width: 100px;
  height: 100px;
  border-radius: 10px;
  object-fit: cover;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.info {
  flex: 1;
}

.info h3 {
  margin: 0;
  font-size: 18px;
  color: #222;
}

.precio {
  margin: 8px 0;
  font-size: 15px;
}

.precio-final {
  color: #e91e63;
  font-weight: bold;
  font-size: 16px;
}

.ver-mas {
  color: #007bff;
  font-size: 14px;
  text-decoration: underline;
  margin-top: 5px;
}

@media screen and (max-width: 600px) {
  .card {
    flex-direction: column;
    align-items: flex-start;
  }

  .card img {
    width: 100%;
    height: auto;
    max-width: 300px;
  }

  .info {
    width: 100%;
  }
}
</style>
