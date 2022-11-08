<script lang="ts" setup>
import {defineProps, defineEmits} from "vue";
import {IPostItem} from "../../type";
import PostItem from "./PostItem.vue";

const props = defineProps<{
  items: IPostItem[]
}>();

const emit = defineEmits(['select-tag']);

function selectTag(tag: string) {
  emit('select-tag', tag);
}
</script>

<template>
  <ul>
    <li v-for="(item, key) in items" :key="key">
      <PostItem :item="item" @select-tag="selectTag" />
    </li>
    <li>
<component is="script" src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-4877378276818686" async></component>
<!-- 수평 디스플레이 광고 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-format="fluid"
     data-ad-layout-key="+3m+oy-47-5p+1aj"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2638932829"></ins>
     <component is="script">
      (adsbygoogle = window.adsbygoogle || []).push({});
     </component>
    </li>
  </ul>
</template>

<style lang="scss" scoped>
@import "../../styles/mixins";

ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}

ul {
  display: flex;
  flex-flow: wrap;
}

li {
  @include only-pc {
    width: calc(100% / 3);
  }
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}
</style>