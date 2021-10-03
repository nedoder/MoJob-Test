<template>
<div>
  <div class="filters" @click="showDropdown()">
    FILTER BY POSITION <i class="arrowDown"></i>
  </div>
  <div id="dropdown" v-if="expanded">
    <div v-for="positionFunction in positionFunctions" :key="positionFunction.id" class="positionFunction">
      <label>
        <input type="checkbox" @change="openCheckboxes(positionFunction.id, $event)" :id="positionFunction.id" :value="positionFunction.name"/>
      {{positionFunction.name}}</label>
      <div v-if="positionFunction.children.length > 0 " class="positionFunctionChildren" :id="'child-'+positionFunction.id">
        <div v-for="children in positionFunction.children" :key="children.id" class="positionChildren" >
          <label>
          <input type="checkbox" :id="children.id" @change="filteringJobs(children.id, $event)" :value="children.name"/>
          {{children.name}}</label><br>
        </div>
      </div>
    </div>
  </div>       
</div>
         
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { JobListing, PositionFunction } from '../models/models';

      
@Component
export default class PositionDropdown extends Vue {
  @Prop({ default: () => [], type: Array as () => PositionFunction[] })
  private positionFunctions!: PositionFunction[];
  public functions: number[] = []
  public expanded: boolean = false;

  openCheckboxes(id:string, value:any):void{
    if (value.target.checked){
      document.getElementById(`child-${id}`)!.style.display = 'block'
      this.functions.push(+id)
    } else {
      document.getElementById(`child-${id}`)!.style.display = 'none'
    }
  }

  filteringJobs(id:string, value:any):void{
    if(value.target.checked){
      this.functions.push(+id)
    }
  }

  showDropdown():void{
    this.expanded = !this.expanded
    if(!this.expanded){
      this.$emit('filterJobs',this.functions)
    }
  }
}

</script>

<style scoped>

.filters {
  width: 380px;
  font-weight: bold;
  text-align: left;
}

.arrowDown {
  border: solid black;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 2px;
  margin: 2px;
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
}

#dropdown {
  width: 300px;
  position: absolute;
  text-align: left;
  background: #FFFFFF;
  z-index: 1 !important;
  box-shadow: 0px 9px 12px rgba(0, 0, 0, 0.14), 0px 3px 16px rgba(0, 0, 0, 0.12), 0px 5px 6px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  padding: 15px 10px;  
}

.positionFunction {
  padding: 5px;
}

.positionFunctionChildren {
    margin-left: 30px;
    padding: 10px 0;
    display: none;
  }

</style>
