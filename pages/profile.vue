<template>
  <div class="container min-h-screen min-w-full mt-6">
    <div v-if="!onEdit" class="flex flex-col gap-4 mx-4 min-w-fit">
      <div class="flex gap-5">
        <img
          :src="
            userData[1]?.publicUrl ||
            userData[0]?.user_metadata?.avatar_url ||
            avatar_default
          "
          :alt="userData[0]?.user_metadata?.full_name"
          class="w-16 h-16 rounded-full object-scale-down"
        />

        <div class="flex flex-col justify-center">
          <h2>{{ userData[0]?.user_metadata?.full_name }}</h2>
          <h4>{{ userData[0]?.user_metadata?.email }}</h4>
        </div>
      </div>
      <button
        class="bg-green-500 p-1 rounded-md hover:opacity-90"
        @click="isEdit()"
      >
        Edit Profile
      </button>
      <hr class="border-t border-gray-500 mx-1 my-5 min-w-fit" />
    </div>
    <div v-else class="flex flex-col gap-4 mx-4 min-w-fix">
      <div class="flex gap-5">
        <img
          :src="
            userData[1]?.publicUrl ||
            userData[0]?.user_metadata?.avatar_url ||
            avatar_default
          "
          :alt="userData[0]?.user_metadata?.full_name"
          class="w-16 h-16 rounded-full object-scale-down"
        />
        <button
          @click="deleteAvatar()"
          class="flex-none absolute p-1 ml-14 mb-6 rounded-full hover:opacity-100 ring-offset-red-500 focus:outline-none focus:ring-1 focus:ring-offset-1"
        >
          <span class="iconify" data-icon="uil:trash"></span>
        </button>
        <div class="flex flex-col justify-center">
          <input
            type="file"
            ref="fileGambar"
            class="block w-full text-sm text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-violet-50 file:text-gray-500 hover:file:bg-violet-100"
            accept="image/*"
          />
        </div>
      </div>

      <button
        class="w-full bg-green-500 rounded-md hover:opacity-90 p-1"
        @click="uploadAvatar"
      >
        Upload
      </button>
    </div>
  </div>
</template>
<script>
import { mapActions, mapState } from 'vuex'
export default {
  middleware: 'supabase-auth',
  data() {
    return {}
  },
  computed: {
    ...mapState(['userData', 'avatarData', 'onEdit', 'avatar_default']),
  },
  methods: {
    ...mapActions([
      'fetchUser',
      'getPublicUrl',
      'avatarUpload',
      'deleteAvatar',
    ]),

    isEdit() {
      this.$store.commit('SET_EDIT', true)
    },

    async fetchUser() {
      await this.$store.dispatch('fetchUser')
    },

    async uploadAvatar() {
      const file = this.$refs.fileGambar.files[0]
      if (!file) {
        return 0
      }
      await this.$store.dispatch('avatarUpload', file)
    },

    async deleteAvatar() {
      await this.$store.dispatch('deleteAvatar')
    },
  },

  updated() {},

  mounted() {
    this.fetchUser()
  },
}
</script>
