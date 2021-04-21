<template>
  <div class="container flex justify-center mt-4">
    <button class="bg-indigo-400 text-white p-2 rounded"
      @click="toggleOpen"
      v-click-outside="toggleOpen"
    >
      Teste
    </button>
  </div>
  <div v-if="open"><p>Abriu</p></div>
</template>

<script>
import { ref } from 'vue'
export default {
  directives: {
    'click-outside': {
      beforeMount(el, binding, vnode) {
        el.clickOutsideEvent = function(event) {
          if (!(el === event.target || el.contains(event.target))) {
            binding.value(event, el);
          }
        }
        document.body.addEventListener('click', el.clickOutsideEvent)
      },
      unmounted(el) {
        document.body.removeEventListener('click', el.clickOutsideEvent)
      }
    }
  },
  setup () {
    const open = ref(false)

    const toggleOpen = () => open.value = !open.value

    return { open, toggleOpen }
  }
}
</script>

<style>

</style>
