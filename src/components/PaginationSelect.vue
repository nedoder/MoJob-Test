<template>
  <div class="pagination">
    <select name="pageSize" id="pageSize" @change="postsPerPage($event)" v-model="paginationNumber">
    <option value="5" selected>5 PER PAGE</option>
    <option value="25">25 PER PAGE</option>
    <option value="500">Display all</option>
  </select>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
      
@Component
export default class PaginationSelect extends Vue {
private paginationNumber: number = 5;

 postsPerPage(value:any): void{
    this.$emit('changePagination', value.target!.value)
  }

}
</script>

<style scoped>

.pagination {
  width: 200px;
  text-align: right;
  background: #FAFDFD;
  z-index: 1 !important;
  border: none;
  padding: 0px 0px 10px;
  font-size: 0.99rem;
}

#pageSize {
  border: none;
  background: #FAFDFD;
  text-align: right;
  font-size: 0.99rem;
  
}


.pagination select {
  width: 100%;
  font-weight: bold;
  border: none;
  outline: none;
}


select option {
  box-shadow: 0px 9px 12px rgba(0, 0, 0, 0.14), 0px 3px 16px rgba(0, 0, 0, 0.12), 0px 5px 6px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  border: none;
  
}


</style>
