<template>
  <div>
    <link-button
      :title="`${showTitle ? 'DM' : ''}`"
      icon-name="email"
      icon-mdi
      @click.native="onDMClick"
    />
    <link-button
      v-if="homeChannelId"
      :title="`${showTitle ? 'ホーム' : ''}`"
      icon-name="home"
      icon-mdi
      @click.native="onHomeChannelClick"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'
import store from '@/store'
import { changeChannelById, changeDMChannelByUsername } from '@/router/channel'
import LinkButton from './LinkButton.vue'

export default defineComponent({
  name: 'Buttons',
  props: {
    homeChannelId: {
      type: String,
      required: false
    },
    userName: {
      type: String,
      required: true
    },
    showTitle: {
      type: Boolean,
      default: false
    }
  },
  setup(props, context) {
    const onDMClick = async () => {
      const nameCache = props.userName
      await store.dispatch.ui.modal.clearModal()
      changeDMChannelByUsername(nameCache)
    }

    const onHomeChannelClick = async () => {
      if (!props.homeChannelId) return
      // モーダル削除時に消えちゃうため、実体を退避
      const idCache = props.homeChannelId
      await store.dispatch.ui.modal.clearModal()
      changeChannelById(idCache)
    }

    return {
      onDMClick,
      onHomeChannelClick
    }
  },
  components: {
    LinkButton
  }
})
</script>
