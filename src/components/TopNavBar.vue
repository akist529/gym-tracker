<template>
  <div class="top-navbar">
    <ModeButton />
    <LogButton :userLoggedIn="userLoggedIn" @click="handleLogButton" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { fetchImages } from '@/mixins/fetchImages'
import ModeButton from '@/components/buttons/ModeButton.vue'
import LogButton from '@/components/buttons/LogButton.vue'
import Cookies from 'js-cookie'

export default defineComponent({
  components: {
    ModeButton,
    LogButton
  },
  mixins: [fetchImages],
  props: {
    userLoggedIn: Boolean
  },
  methods: {
    handleLogButton () {
      if (Cookies.get('token')) {
        this.$emit('openWarningModal')
      } else {
        this.$emit('openLoginModal')
      }
    }
  }
})
</script>

<style scoped lang="scss">
.top-navbar {
  grid-row: 1 / 2;
  grid-column: 1 / -1;

  padding: 10px 5px;
  width: 100%;

  background: rgb(20, 20, 20);

  display: flex;
  justify-content: space-between;
  align-items: center;

  img {
    filter: invert(1);
  }
}
</style>
