<template>
    <div>
      <h2>Iniciar Sesión</h2>
      <form @submit.prevent="login">
        <input v-model="credentials.email" placeholder="Correo" type="email" />
        <input v-model="credentials.password" placeholder="Contraseña" type="password" />
        <button type="submit">Ingresar</button>
      </form>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import axios from 'axios';
  import { useRouter } from 'vue-router';
  
  export default {
    setup() {
      const router = useRouter();
      const credentials = ref({ email: '', password: '' });
  
      const login = async () => {
        try {
          const { data } = await axios.post('/api/login', credentials.value);
          localStorage.setItem('token', data.token);
          router.push('/');
        } catch (error) {
          alert('Credenciales inválidas');
        }
      };
  
      return { credentials, login };
    }
  };
  </script>
  