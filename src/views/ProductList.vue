<template>
  <div>
    <h2>Listado de Productos</h2>
    <router-link to="/product">
      <button class="btn">Agregar Producto</button>
    </router-link>

    <!-- Verifica si hay productos antes de mostrar la tabla -->
    <div v-if="loading">Cargando...</div>
    <table v-else>
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Descripcion</th>
          <th>Precio</th>
          <th>Stock</th>
          <th>Precio IVA</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products.data" :key="product.id">
          <td>{{ product.name }}</td>
          <td>{{ product.description }}</td>
          <td>{{ product.price }}</td>
          <td>{{ product.stock }}</td>
          <td>{{ product.price_with_vat }}</td>
         
          <td>
            <router-link :to="`/product/${product.id}`">
              <button class="btn-edit">Editar</button>
            </router-link>
            <button class="btn-delete" @click="deleteProduct(product.id)">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Paginación -->
    <div v-if="products.meta && products.meta.total > 10">
      <button v-if="products.meta.current_page > 1" @click="getPage(products.meta.current_page - 1)">
        Anterior
      </button>
      <button v-if="products.meta.current_page < products.meta.last_page" @click="getPage(products.meta.current_page + 1)">
        Siguiente
      </button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const products = ref({});
    const loading = ref(true); // Variable para controlar el estado de carga

    // Cargar productos desde la API
    const fetchProducts = async () => {
      try {
        const response = await axios.get('http://localhost:8000/api/products'); 
        products.value = response.data;
      } catch (error) {
        console.error('Error al obtener los productos', error);
      } finally {
        loading.value = false; // Desactivar el estado de carga
      }
    };

    // Cargar productos de una página específica
    const getPage = async (page) => {
      try {
        const response = await axios.get(`http://localhost:8000/api/products?page=${page}`);
        products.value = response.data;
      } catch (error) {
        console.error('Error al obtener los productos', error);
      }
    };

    // Eliminar producto
    const deleteProduct = async (id) => {
      if (confirm('¿Estás seguro de que deseas eliminar este producto?')) {
        try {
          await axios.delete(`http://localhost:8000/api/products/${id}`);
          products.value.data = products.value.data.filter(product => product.id !== id);
        } catch (error) {
          console.error('Error al eliminar el producto', error);
        }
      }
    };

    onMounted(fetchProducts);

    return { products, loading, getPage, deleteProduct };
  }
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
}

th {
  background-color: #333;
  color: white;
}

.btn {
  background-color: #4CAF50;
  color: white;
  padding: 10px;
  border: none;
  cursor: pointer;
  margin-bottom: 10px;
}

.btn-edit {
  background-color: #2196F3;
  color: white;
  padding: 5px;
  border: none;
  cursor: pointer;
  margin-right: 5px;
}

.btn-delete {
  background-color: #f44336;
  color: white;
  padding: 5px;
  border: none;
  cursor: pointer;
}
</style>
