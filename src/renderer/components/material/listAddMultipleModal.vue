<template lang="pug">
material-modal(:show="show" :bg-close="bgClose" @close="handleClose")
  main(:class="$style.main")
    h2 {{$t('material.list_add_multiple_modal.title', { num: musicList.length })}}
    material-btn(:class="$style.btn" :title="$t('material.list_add_multiple_modal.btn_title', { name: item.name })" :key="item.id" @click="handleClick(index)" v-for="(item, index) in lists") {{item.name}}
</template>

<script>
import { mapGetters, mapMutations } from 'vuex'
export default {
  props: {
    show: {
      type: Boolean,
      default: false,
    },
    musicList: {
      type: Array,
      default() {
        return []
      },
    },
    bgClose: {
      type: Boolean,
      default: true,
    },
    excludeListId: {
      type: Array,
      default() {
        return []
      },
    },
  },
  computed: {
    ...mapGetters('list', ['defaultList', 'loveList', 'userList']),

    lists() {
      return [
        this.defaultList,
        this.loveList,
        ...this.userList,
      ].filter(l => !this.excludeListId.includes(l.id))
    },
  },
  methods: {
    ...mapMutations('list', ['listAddMultiple']),
    handleClick(index) {
      this.listAddMultiple({ id: this.lists[index].id, list: this.musicList })
      this.$nextTick(() => {
        this.handleClose(true)
      })
    },
    handleClose(isSelect = false) {
      this.$emit('close', isSelect)
    },
  },
}
</script>


<style lang="less" module>
@import '../../assets/styles/layout.less';

.main {
  padding: 15px;
  max-width: 300px;
  min-width: 200px;
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  h2 {
    font-size: 13px;
    color: @color-theme_2-font;
    line-height: 1.3;
    text-align: center;
    margin-bottom: 15px;
  }
}

.btn {
  display: block;
  margin-bottom: 15px;
  &:last-child {
    margin-bottom: 0;
  }
}

each(@themes, {
  :global(#container.@{value}) {
    .main {
      h2 {
        color: ~'@{color-@{value}-theme_2-font}';
      }
    }
  }
})

</style>
