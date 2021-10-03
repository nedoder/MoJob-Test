<template>
  <div class="home">
    <div class="logo">
    <img alt="Vue logo" src="../assets/mojob_logo_on_white.png" />
    </div>
    <div class="wrapper">
    <div class="flex">
    <position-dropdown
      :position-functions="this.positionFunctionFilters"  
      @filterJobs = 'filterData($event)' 
    />
    <pagination-select @changePagination = 'postsPerPage($event)'/>
    </div>
    </div>
    <job-feed
      v-for="job in this.jobListing"
      :job-listing="job"
      :key="job.id"
    />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import JobFeed from '../components/JobFeed.vue';
import PositionDropdown from '../components/PositionFunction.vue';
import PaginationSelect from '../components/PaginationSelect.vue';
import BaseApi from '../api-requests/api';
import { IPage, PositionFunction, JobListing } from '../models/models';

@Component({
  components: {
    JobFeed,
    PositionDropdown,
    PaginationSelect,
  },
})
export default class Home extends Vue {
  private mojobApi: BaseApi | null = null;
  private positionFunctionFilters: PositionFunction[] = [];
  private jobListing: JobListing[] = [];
  private paginationNumber: number = 5;
  
  private async mounted() {
    this.mojobApi = new BaseApi(
      'https://test-api.mojob.io/public/',
       this.axios
    );
    try {
      const jobListingsResponsePage: IPage<JobListing> =
        await this.mojobApi.getJobListings(5);
      if(jobListingsResponsePage.results) {
        this.jobListing = jobListingsResponsePage.results;
      } else {
        console.log(jobListingsResponsePage);
      }
    } catch(e) {
      console.log('Failed loading job listing');
      console.log(e);
    }
  
    try {
      const jobLocationFiltersResponsePage: IPage<PositionFunction> =
        await this.mojobApi.getPositionFunctions();
      if(jobLocationFiltersResponsePage.results) {
        this.positionFunctionFilters = jobLocationFiltersResponsePage.results;
      } else {
        console.log('Failed loading position function filters');
      }
    } catch(e) {
      console.log('Failed loading position function filters');
      console.log(e);
    }
  }

  async postsPerPage (value: number){
    this.paginationNumber = value
    this.mojobApi = new BaseApi(
      'https://test-api.mojob.io/public/',
      this.axios
    );
    try {
      const jobListingsResponsePage: IPage<JobListing> = await this.mojobApi.getJobListings(+value);
      if(jobListingsResponsePage.results) {
        this.jobListing = jobListingsResponsePage.results;
      }
    } catch(e) {
      console.log('Failed loading job listing');
      console.log(e);
    }
  }

  async filterData(value: number[]) {
    const filters:string = value.join(',')
    this.mojobApi = new BaseApi(
      'https://test-api.mojob.io/public/',
      this.axios
    );
    try {
      let jobListingsResponsePage: IPage<JobListing>;
      if(value.length>0){
        jobListingsResponsePage = await this.mojobApi.getJobListings(this.paginationNumber, filters);
      } else {
        jobListingsResponsePage = await this.mojobApi.getJobListings(this.paginationNumber);
      }
      if(jobListingsResponsePage.results) {
        this.jobListing = jobListingsResponsePage.results;
      }
    } catch(e) {
      console.log('Failed loading job listing');
      console.log(e);
    }
  }

}

</script>


<style scoped>

.home {
  width: 100%;
}
.logo {
  text-align: left;
}
img {
  width: 120px;
  height: 120px;
}

.flex {
  display: flex;
  width: 80%;
  margin: 0 auto;
  padding: 10px;
  justify-content: space-between;
}

.wrapper {
  width: 100%;
  background: #FAFDFD;
  padding-top: 20px;
}

</style>