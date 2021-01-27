<template>
  <div>
    <l-n-b></l-n-b>
    <my-header>
    </my-header>
    <billboard></billboard>
    <direct></direct>
    <hot-focus></hot-focus>
  </div>
</template>

<script>
import LNB from '~/src/components/LNB'
import MyHeader from "~/src/components/MyHeader"
import Billboard from "~/src/components/Billboard";
import Direct from "~/src/components/Direct";
import HotFocus from "~/src/components/HotFocus";

export default {
  components: {
    LNB,
    MyHeader,
    Billboard,
    Direct,
    HotFocus
  }
}

</script>

<style lang="scss">
  @import "./scss/main.scss";


</style>