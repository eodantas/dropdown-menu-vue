<template>
  <div class="min-h-screen bg-indigo-100 flex justify-end p-4">
    <div class="relative">
      <button
        @click="toggleOpen"
        @keyup.esc.exact="toggleOpen"
        @keyup.down.exact="downPressed"
        @keyup.up.exact="upPressed"
        @keydown.shift.tab.exact="toggleOpen"
        v-click-outside="toggleOpen"
        class="inline-flex justify-center w-full rounded-md border
          border-gray-300 shadow-sm px-4 py-2 bg-white text-sm font-medium
          text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2
          focus:ring-offset-2 focus:ring-offset-gray-100 focus:ring-indigo-500
        "
      >
        Options Menu
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"
           class="-mr-1 ml-2 h-5 w-5"
           aria-hidden="true"
        >
          <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
        </svg>
      </button>
      <transition name="fade">
        <div v-if="isOpen" ref="menuDrop"
          class="origin-top-right absolute right-0 mt-2 w-56 rounded-md shadow-lg
            bg-white ring-1 ring-black ring-opacity-5 divide-y divide-gray-100
            focus:outline-none
          "
        >
          <div v-for="(dropItem, index) in dropItems" :key="index" class="py-1">
            <a v-for="item in dropItem" :key="item.text" href="#"
              @mouseover="mouseOver(item.index)"
              class="group flex items-center px-4 py-2 text-sm menu-item"
              :class="{
                'text-white': item.index == actualIndex,
                'bg-indigo-500': item.index == actualIndex,
                'text-gray-700': item.index != actualIndex && !item.disabled,
                'text-gray-300': item.disabled,
                'hover:bg-indigo-500': actualIndex == -1 && !item.disabled,
                'hover:text-white': actualIndex == -1 && !item.disabled
              }"
              :disabled="item.disabled"
            >
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"
                class="mr-3 h-5 w-5"
                :class="{
                  'text-white': item.index == actualIndex,
                  'text-gray-400': item.index != actualIndex && !item.disabled,
                  'text-gray-300': item.disabled,
                  'group-hover:text-white': actualIndex == -1 && !item.disabled
                }"
                aria-hidden="true"
                v-html="item.icon"
              >
              </svg>
              {{ item.text }}
            </a>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import { computed, ref } from 'vue'
export default {
  setup () {
    // DATA
    const dropItems = ref([
      [
        {
          icon: '<path d="M17.414 2.586a2 2 0 00-2.828 0L7 10.172V13h2.828l7.586-7.586a2 2 0 000-2.828z" /><path fill-rule="evenodd" d="M2 6a2 2 0 012-2h4a1 1 0 010 2H4v10h10v-4a1 1 0 112 0v4a2 2 0 01-2 2H4a2 2 0 01-2-2V6z" clip-rule="evenodd" />',
          text: 'Edit',
          index: 0,
          disabled: false
        },
        {
          icon: '<path d="M7 9a2 2 0 012-2h6a2 2 0 012 2v6a2 2 0 01-2 2H9a2 2 0 01-2-2V9z" /><path d="M5 3a2 2 0 00-2 2v6a2 2 0 002 2V5h8a2 2 0 00-2-2H5z" />',
          text: 'Duplicate',
          index: 1,
          disabled: true
        }
      ],
      [
        {
          icon: '<path d="M4 3a2 2 0 100 4h12a2 2 0 100-4H4z" /><path fill-rule="evenodd" d="M3 8h14v7a2 2 0 01-2 2H5a2 2 0 01-2-2V8zm5 3a1 1 0 011-1h2a1 1 0 110 2H9a1 1 0 01-1-1z" clip-rule="evenodd" />',
          text: 'Archive',
          index: 2,
          disabled: false
        },
        {
          icon: '<path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-8.707l-3-3a1 1 0 00-1.414 1.414L10.586 9H7a1 1 0 100 2h3.586l-1.293 1.293a1 1 0 101.414 1.414l3-3a1 1 0 000-1.414z" clip-rule="evenodd" />',
          text: 'Move',
          index: 3,
          disabled: false
        }
      ],
      [
        {
          icon: '<path d="M8 9a3 3 0 100-6 3 3 0 000 6zM8 11a6 6 0 016 6H2a6 6 0 016-6zM16 7a1 1 0 10-2 0v1h-1a1 1 0 100 2h1v1a1 1 0 102 0v-1h1a1 1 0 100-2h-1V7z" />',
          text: 'Share',
          index: 4,
          disabled: false
        },
        {
          icon: '<path fill-rule="evenodd" d="M3.172 5.172a4 4 0 015.656 0L10 6.343l1.172-1.171a4 4 0 115.656 5.656L10 17.657l-6.828-6.829a4 4 0 010-5.656z" clip-rule="evenodd" />',
          text: 'Add to favorites',
          index: 5,
          disabled: false
        }
      ],
      [
        {
          icon: '<path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />',
          text: 'Delete',
          index: 6,
          disabled: false
        }
      ]
    ])
    const isOpen = ref(false)
    const menuDrop = ref(null)
    const actualIndex = ref(-1)
    let mouseIndex = -1

    // COMPUTEDS
    const items = computed(() => {
      if (menuDrop.value != null) {
        return menuDrop.value.getElementsByClassName('menu-item')
      }
      return []
    })
    const totalItems = computed(() => items.value.length)

    // METHODS
    const toggleOpen = () => isOpen.value = ! isOpen.value
    const downPressed = () => {
      if (isOpen.value) {
        if (mouseIndex != -1) {
          actualIndex.value = mouseIndex
          mouseIndex = -1
        }
        actualIndex.value++
        if (actualIndex.value == totalItems.value) {
          actualIndex.value = 0
        }
        for (let i = actualIndex.value; i < totalItems.value; i++) {
          if (items.value[i].getAttribute('disabled') == 'true') {
            actualIndex.value++
            if (actualIndex.value == totalItems.value) {
              actualIndex.value = 0
            }
          } else {
            break
          }
        }
      }
    }
    const upPressed = () => {
      if (isOpen.value) {
        if (mouseIndex != -1) {
          actualIndex.value = mouseIndex
          mouseIndex = -1
        }
        actualIndex.value--
        if (actualIndex.value < 0) {
          actualIndex.value = totalItems.value - 1
        }
        for (let i = actualIndex.value; i >= 0; i--) {
          if (items.value[i].getAttribute('disabled') == 'true') {
            actualIndex.value--
            if (actualIndex.value < 0) {
              actualIndex.value = totalItems.value - 1
            }
          } else {
            break
          }
        }
      }
    }
    const mouseOver = index => {
      mouseIndex = index
      actualIndex.value = -1
    }

    return {
      dropItems, isOpen, menuDrop, actualIndex, items, totalItems, toggleOpen,
      downPressed, upPressed, mouseOver
    }
  }
}
</script>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity .3s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
