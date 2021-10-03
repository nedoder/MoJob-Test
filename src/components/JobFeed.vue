<template>
  <div class="job-feed">
    <div class="card-feed">
        <div>
          <h4>{{jobListing.job.title}}</h4>
        </div>
        <div class="flex">
          <p>{{jobListing.job.unit.display_name}}</p>
          <p>{{jobListing.job.position_function.name_en}}</p>
          <p>{{jobListing.job.due_date}}</p>
        </div>
    </div> 
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { JobListing, PositionFunction } from '../models/models';

@Component
export default class JobFeed extends Vue {
  @Prop({ default: () => [], type: Object as () => JobListing[] })
  private jobListing!: JobListing[];
}
</script>

<style scoped>

.job-feed {
  background: #FAFDFD;
  display: flex;
  flex-direction: column;
}

.card-feed {
  background: #FFFFFF;
  width: 80%;
  height: 84px;
  border-radius: 10px;
  text-align: left;
  padding: 15px;
  margin: 15px auto;
  box-shadow: 0px 4px 5px rgba(0, 0, 0, 0.05), 0px 1px 10px rgba(0, 0, 0, 0.08), 0px 2px 4px rgba(0, 0, 0, 0.14);
}

.flex {
    display: flex;
    flex-direction: row;
}

h4 {
    font-size: 20px;
    margin: 10px 0;
    color: #2E2E2E;
    line-height: 27px;
    letter-spacing: 0.25px;
    font-weight: 700;
}

p {
    font-size: 16px;
    margin: 5px 10px;
    color: #757575;
}

p:first-child {
    margin-left: 0;
}


p::after{
  content: '.';
  position: absolute;
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background-color: #757575;
  margin: 0 5px;
  margin-top: 6px
}

p:last-child::after {
    content: '';
    background-color: transparent;
}

</style>
