<template>
  <div class="main">
    <h1> {{ msg }} </h1>

    <table>
      <tr>
        <td colspan="1" align="left">
          搜尋: <input type="text" @change="flush" v-model="keyword" placeholder="keyword">
        </td>
      </tr>
      <tr>
        <td>
          <table border="1" width="100%" class="comicGreen">
            <thead>
              <tr>
                <th width="120">日期</th>
                <th>名稱</th>
                <th width="120">擁有者</th>
                <th>介紹</th>
                <th>標籤</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td><input type="text" class="qfield" @change="flush" v-model="qdate" placeholder="date"></td>
                <td><input type="text" class="qfield" @change="flush" v-model="qproject" placeholder="project"></td>
                <td><input type="text" class="qfield" @change="flush" v-model="qowner" placeholder="owner"></td>
                <td><input type="text" class="qfield" @change="flush" v-model="qdescription" placeholder="description"></td>
                <td><input type="text" class="qfield" @change="flush" v-model="qtag" placeholder="tag"></td>
              </tr>
              <tr v-for="item in items" :key="item" @click="showItem(item)">
                <td :title='item["Date"]'>{{ item["Date"] }}</td>
                <td :title='item["Project"]'><a :href='item["Link"]' target='_blank'>{{ item["Project"] }}</a></td>
                <td :title='item["Owner"]'>{{ item["Owner"] }}</td>
                <td :title='item["Description"]'>{{ item["Description"] }}</td>
                <td :title='item["Tags"]'>{{ item["Tags"] }}</td>
              </tr>
            </tbody>
          </table>
        </td>
      </tr>
      <tr>
        <td align="right">
          開發者: 骨董
        </td>
      </tr>
    </table>

  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import axios from 'axios'

@Component
export default class Main extends Vue {
  @Prop() private msg!: string;
  
  selected: string = "";
  all_items: any[] = [];
  items: any[] = [];
  keyword: string = "";
  qdate: string = "";
  qproject: string = "";
  qowner: string = "";
  qdescription: string = "";
  qtag: string = "";
  
  isEmpty(v: string) {
    return v === undefined || !v || !v.trim();
  }

  check(keyword: string, text: string) {
      if (keyword.length > 0 && (this.isEmpty(text) || text.indexOf(keyword) == -1)) {
        return true;
      } else {
        return false;
      }
  }

  flush() {
    let candidates: any[] = [];
    this.all_items.filter( item => {
      if (this.keyword.length > 0) {
        if (this.check(this.keyword, item.Date) &&
            this.check(this.keyword, item.Project) &&
            this.check(this.keyword, item.Owner) &&
            this.check(this.keyword, item.Description) &&
            this.check(this.keyword, item.Tags)) {
            return;
        }
      }

      if (this.check(this.qdate, item.Date)) return;
      if (this.check(this.qproject, item.Project)) return;
      if (this.check(this.qowner, item.Owner)) return;
      if (this.check(this.qdescription, item.Description)) return;
      if (this.check(this.qtag, item.Tags)) return;

      candidates.push(item);
    })

    candidates.reverse()
    this.items = [...candidates];
  }

  mounted() {
    // https://sheets.googleapis.com/v4/spreadsheets/1aNK5mM4Y3XuWRt-8gHokR411qLPw_6bXucmKupO_heU/values/sheet1!A1:AB1000?key=AIzaSyBhiqVypmyLHYPmqZYtvdSvxEopcLZBdYU

    let vm = this;
    axios.get(
                'https://sheets.googleapis.com/v4/spreadsheets/1C9-g1pvkfqBJbfkjPB0gvfBbBxVlWYJj6tTVwaI5_x8/values/%E5%A4%A7%E6%9D%BE%E6%8F%90%E6%A1%88%E5%88%97%E8%A1%A8!A1:W10000?key=AIzaSyBhiqVypmyLHYPmqZYtvdSvxEopcLZBdYU'
            )
            .then(function (response) {
                let specials = response.data.values
                for (let index = 1; index < specials.length; index++) {
                    const element = specials[index]
                    let mitem = {
                        Date: element[0],
                        Project: element[3],
                        Owner: element[11],
                        Description: element[4],
                        Link: element[6],
                        Tags: element[26]
                    }
                    vm.all_items.push(mitem);
                }

                vm.flush();
                //console.log(response)
            })
  }

  showItem(item: any) {
    this.selected = item
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@import "../assets/home.less";
</style>
