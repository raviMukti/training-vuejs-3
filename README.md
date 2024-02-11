# Vue Composition API Basic

1. Option-API selalu diawali dengan kode berikut
```
<script>
    export default {
        data() {
            // data atau variable
        },
        methods: {
            // method/function
        }
    }
</script>
```

2. Composition-API ada 2 jenis gaya untuk penulisan kode dengan Composition-API
    1. Composition-API Setup
    ```
    <script>
        import { ref } from 'vue'

        export default {
            setup() {
                const counter = ref(0)

                const increaseCounter = () => {
                    counter.value++
                }

                const decreaseCounter = () => {
                    counter.value--
                }

                return {
                    counter,
                    increaseCounter,
                    decreaseCounter
                }
            }
        }
    </script>
    ```
    2. Composition-API Script-Setup
    ```
        <script setup>
            import { ref } from 'vue'

            const counter = ref(0)

            const increaseCounter = () => {
                counter.value++
            }

            const decreaseCounter = () => {
                counter.value--
            }
        </script>
    ```
3. Tipe data pada Composition-API
    1. Refs adalah tipe data sederhana yang hanya digunakan ketika akan menampung 1 jenis informasi saja
    ```
        <script setup>
            import { ref } from 'vue'

            const counter = ref(0)

            const increaseCounter = () => {
                counter.value++
            }

            const decreaseCounter = () => {
                counter.value--
            }
        </script>
    ```
    2. Reactive Object adalah tipe data yang digunakan untuk mendeklarasikan data dengan jumlah attribut yang banyak dan kompleks
    ```
    <script setup>
    import { reactive } from 'vue'

    const appTitle = 'My Ok Counter App'

    const counterData = reactive({
        count: 0,
        title: 'My Counter'
    })

    // methods
    const increaseCounter = (amount, e) => {
        counterData.count += amount
    }

    const decreaseCounter = amount => {
        counterData.count -= amount
    }
    </script>
    ```
    3. Non-Reactive Object ini adalah tipe data yang digunakan jika kamu menginginkan sebuah value/nilai pada komponen tidak sering berubah dan tidak diubah secara programmatikal, contohnya diatas bisa diimplementasikan langsung di variable appTitle

4. Methods adalah sebuah function yang dideklarasikan untuk melakukan perubahan pada dom ataupun variable, sama seperti dibahasa pemrograman lain, methods juga dapat menerima parameter input

5. Computed properties adalah sebuah properti/attribut yang biasanya dibuat berdasarkan reactive data, ter-cached serta hanya terupdate jika dependency berubah, contoh jika kita akan membuat sebuah computed properties, kita bisa mengambil dari reactive-object counterData.count dan jika ada perubahan pada attribut itu, maka value computed properties juga akan ikut berubah
```
<script setup>
import { reactive, computed } from 'vue'

const appTitle = 'My Ok Counter App'

const counterData = reactive({
  count: 0,
  title: 'My Counter'
})


const oddOrEven = computed(() => {
  if (counterData.count % 2 === 0) return 'even'
  return 'odd'
})

const increaseCounter = (amount, e) => {
  counterData.count += amount
}

const decreaseCounter = amount => {
  counterData.count -= amount
}
</script>
```

6. Watchers secara sederhana membuat kita untuk bisa menyaksikan reactive-data properties dan melakukan sesuatu ketika data yang disaksikan ada perubahan
```
<script setup>
import { reactive, computed, watch } from 'vue'

const appTitle = 'My Ok Counter App'

const counterData = reactive({
  count: 0,
  title: 'My Counter'
})

watch(() => counterData.count, (newCount) => {
  if (newCount === 20) {
    alert('Way to go! You made it to 20!!')
  }
})

const oddOrEven = computed(() => {
  if (counterData.count % 2 === 0) return 'even'
  return 'odd'
})

const increaseCounter = (amount, e) => {
  counterData.count += amount
}

const decreaseCounter = amount => {
  counterData.count -= amount
}
</script>
```

7. Router pada vue 3 jika kita menggunakan Composition-API kita tidak bisa mengakses langsung menggunakan `this` dan juga tidak bisa mengakses `vue-instance` maka dari itu kita harus menggunakan `useRoute` dan juga `useRouter` yang berasal dari `vue import`

8. Directives adalah function bawaan dari vue, ada banyak macamnya, contoh jika kita ingin melakukan rendering ke template secara kondisional kita bisa menggunakan `v-if` atau jika kita ingin melakukan loop terhadap sebuah reactive-object/data kita bisa menggunakan `v-for`

9. Template-refs adalah sebuah fitur yang memungkinkan kita bisa mengakses sebuah element html dan menjadikannya sebagai sebuah variable, dan kita bisa mengubah variable yang ada pada element tersebut
```
<template>
    ...
    <h2 ref="appTitleRef">Title</h2>
    ...
</template>

<script setup>
    const appTitleRef = ref(null)
</script>

```

10. nextTick mengizinkan kita untuk menunggu sampai dom selesai diupdate dan melakukan sesuatu

11. Teleport mengizinkan kita untuk memindahkan sebuah element dari lokasi default di dom ke lokasi lain pada dom, ini bisa berguna ketika kita akan membuat modal
```
<template>
  <div class="modals">
    <h1>Modals</h1>
    <button @click="showModal = true">Show modal</button>
    <teleport to=".modals-container">
      <div
        v-if="showModal"
        class="modal"
      >
        <h1>This is a modal</h1>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Beatae ipsa laboriosam vero natus ut rerum quaerat, saepe praesentium tempore et hic velit odio nemo minus labore quam ullam quod architecto?</p>
        <button @click="showModal = false">Hide modal</button>
      </div>
    </teleport>
  </div>
</template>

<script setup>
/*
  imports
*/

  import { ref } from 'vue'

/*
  modals
*/

  const showModal = ref(false)

</script>

<style>
.modal {
  background: beige;
  padding: 10px;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}
</style>
```

12. Slots bisa digunakan untuk menampung nilai pada sebuah elemen child yang dikirim dari parent elemen
```
<template>
    <h1>Title</h1>
    <slot />
</template>
```

13. Props adalah sebuah properti atau attribut yang bisa digunakan untuk mengirimkan value/nilai dari parent ke child atau antar component
```
// Parent
<template>
    <Modal 
        title = "My Title"
    >
    </Modal>
</template>

// Child
<template>
    <h1>{{ props.title }}</h1>
    <slot />
</template>

<script setup>
    const props = defineProps({
        title: {
            type: String,
            default: 'No title defined'
        }
    })
</script>
```

14. Emits adalah cara untuk memancarkan event/kejadian tertentu dari child component/component lain ke parent component, sehingga event yang terjadi pada child dapat diketahui oleh parent component, dan pada parent component dapat melakukan sesuatu
```
// Child
<template>
    <h1>{{ props.title }}</h1>
    <slot />
    <button @click="$emit('hideModal')">Hide Modal</button>
</template>

<script setup>
    const emit = defineEmits(['hideModal'])
</script>


// Parent
<template>
    <Modal
        title="My Title"
        @hideModal="showModal = false"
    >
    </Modal>
</template>
```

15. modalValue adalah alternatif lain jika kita ingin mengubah sebuah nilai atau variable yang ada di parent dari child nya secara langsung, alih - alih menggunakan emit dimana kita harus mengirim event dan melisten event tersebut
```
// Parent
<template>
    <Modal
        v-model="showModal"
        title="My Title"
    >
    </Modal>
</template>

// Child
<template>
    <h1>{{ props.title }}</h1>
    <slot />
    <button @click="$emit('hideModal')">Hide Modal</button>
</template>

<script setup>
    const props = defineProps({
        modelValue: {
            type: Boolean,
            default: false
        },
        title: {
            type: String,
            default: 'No title defined'
        }
    })
</script>
```

16. Dynamic component memungkinkan kita untuk mengganti komponen yang digunakan pada aplikasi kita.

17. Provide/Inject memungkinkan kita untuk mem-passing data dari komponen a ke komponen lain. Sebetulnya bisa saja kita menggunakan props, akan tetapi jika hirarki componentnya sangat berjaring/nested maka ini akan sangat menyulitkan, nah dengan adanya provide/inject ini setidaknya masalahnya bisa langsung teratasi.

18. Composables memungkinkan kita membuat sebuah class dengan function yang bisa digunakan dimanapun, composables mampu mengubah reactive data dan juga function tertentu

19. State management memudahkan kita dalam mengambil dan juga mengubah data yang harus tersedia secara global, dan kita juga dapat mengakses attribute ataupun method yang ada pada data tersebut. Tempat dimana data tersebut disimpan dinamakan store. Pinia adalah salah satu tools yang digunakan untuk state management di vue, ada alternatif lain salah satunya adalah vuex. Ada beberapa bagian pada Pinia, pertama adalah state, getters, dan actions. State merupakan function/method dimana kita mendifinisikan attribut/properti dari data yang akan kita buat. Actions adalah method yang dapat mengakses state yang sudah kita buat. Dan getters bayangkan ini adalah computed properties.