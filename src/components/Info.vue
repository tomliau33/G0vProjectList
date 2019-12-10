<template>
  <div class="info">
    <table border="1" class="comicRed">
    <thead>
        <tr>
        <th width='200'>欄位名稱</th>
        <th>內容</th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="(info, index) in infos" :key="info">
            <td align='center'>{{ fields[index] }}</td>
            <td v-if='info.startsWith("http")'><a :href='info' target='_blank'>{{ info }}</a></td>
            <td v-else>{{ info }}</td>
        </tr>
    </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from 'vue-property-decorator';

export default {
    props: {
        fields: [],
        infos: []
    },
    data() {
        return {
        }
    },
    mounted() {
    }
}

</script>

<style scoped lang="less">
@import "../assets/info.less";
</style>
