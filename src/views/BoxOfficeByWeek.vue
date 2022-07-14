<template>
    <div>
    <h1>{{title}}</h1>
    <div>
      <input type="date" v-model="selectedDate">
      <button @click="search">검색</button>
    </div>
    <rank-table :list="list" />
  </div>
</template>

<script>
import RankTable from '../components/boxoffice/RankTable';

export default {
  components: {
    RankTable
  },
  data(){
    return {
      title: '주간 박스오피스',
      selectedDate: '',
      list: []
    }
  },
  methods: {
    search(){
      const targetDt = this.selectedDate.replaceAll('-', '');
      this.getData(targetDt);
    },
    async getData(targetDt) {
        const data = await this.getBoxOfficeByWeek(targetDt, '0');
        this.list = data.boxOfficeResult.weeklyBoxOfficeList;

        const capYearWeek = document.querySelector('#yearWeekTime');
        const year = data.boxOfficeResult.yearWeekTime.substr(0, 4);
        const week = data.boxOfficeResult.yearWeekTime.substr(4, 2);
        capYearWeek.innerText = `${year}년 ${week}주차`;
    }
  },
  created(){
    const d = new Date();
    d.setDate(d.getDate() - 1);
    this.selectedDate = this.getOnlyDateStr(d);
  }
}
</script>

<style>

</style>