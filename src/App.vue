<script>
import { RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'
import FooterComponent from './components/FooterComponent.vue'
import axios from 'axios'

export default {
  name: "App",
  components: {
    Navbar,
    RouterView,
    FooterComponent
  },
  data() {
    return {
      data: ''
    }
  },
  methods: {
    async getData() {
      try {
        const data = await axios.get("http://localhost:3000/api/v1/cryptocurrency/listings/latest", {
          headers: {
            'X-CMC_PRO_API_KEY': '000470f2-3058-4f7a-88a8-b944668f4e89'
          }
        })
        this.data = data.data.data
      } catch (error) {
        console.log(error)
      }
    },
  },
  mounted() {
    this.getData()
  },
  provide() {
    return {
      getData: () => this.data
    }
  }
}

</script>

<template>
  <Navbar />
  <RouterView v-slot="slotProps">
    <transition name="route" mode="out-in">
      <component :is="slotProps.Component"></component>
    </transition>
  </RouterView>
  <FooterComponent />
</template>

<style>
@import '@/assets/base.css';

.main-container {
  max-width: 1200px;
  margin: 0 auto;
}

.flex {
  display: flex;
}

.column {
  flex-direction: column;
}

.align-center {
  align-items: center;
}

.text-align-center {
  text-align: center;
}

.justify-center {
  justify-content: center;
}

.space-between {
  justify-content: space-between;
}

li {
  list-style: none;
}

a {
  text-decoration: none;
}

.input input {
  padding: 10px;
  border: 1px solid rgb(216, 216, 216);
  outline-color: lightblue;
  margin-bottom: 10px;
}

.route-enter-active {
  animation: anim .5s ease-out;
}

.route-leave-active {
  animation: anim .5s ease-in reverse;
}

@keyframes anim {
  0% {
    opacity: 0;
    transform: translateX(-30px);
  }

  100% {
    opacity: 1;
    transform: translateX(0);
  }
}
</style>
