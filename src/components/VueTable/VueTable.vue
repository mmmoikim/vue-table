<template>
<table>
  <thead>
    <tr>
      <th v-if="isHandler" style="width : 20px">〓</th>
      <th v-for="(head , index) in header" :key="index" :ref="'header_' + index" @dragover="onDragOverResizer">
        <div v-if="index != 0" draggable @dragstart="onDragStartResizer(index - 1, $event)" class="resizer_left">&nbsp;</div>
        {{head.Header}}
        <div v-if="index != (header.length - 1)" draggable @dragstart="onDragStartResizer(index, $event)" class="resizer_right">&nbsp;</div>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(obj , trIndex) in originData" :key="'tr_'+trIndex">
      <td v-if="isHandler" draggable="true" @dragstart="onHandelerDragStart(obj)" @dragover="onHandelerDragOver">〓</td>
      <td v-for="(head , tdIndex) in header" :key="'td_'+tdIndex">{{obj[head.accessor]}}</td>
    </tr>
       </tbody>
</table>
</template>

<script>
export default {
  name: 'VueTable',
  data(){
    return {
      originData : this.tableData,
      dragColumn : null,
      dragIndex : null,
      resizerStartOffset : null,
      resizerIndex : null,
      headerWidth : []

    }
  },
  created(){
  },
  mounted(){
    this.headerWidth = [];
    this.header.map((obj, index)=>{
      this.headerWidth.push(this.$refs['header_' + index][0].offsetWidth);
    })
  },
  watch:{
  },
  methods:{
    onHandelerDragStart(obj){
      this.dragColumn = obj;
    },
    onHandelerDragOver(event){
      event.preventDefault();
      let eventSequence = event.currentTarget.parentNode.rowIndex - 1;
      let dragIndex = this.originData.indexOf(this.dragColumn);
      if(this.dragColumn && eventSequence != dragIndex){
         let clone = [...this.originData];
         clone[dragIndex] = clone[eventSequence];
         clone[eventSequence] = this.dragColumn;
         this.originData = clone;
      }
    },
    //resizer
    onDragStartResizer(index, event){
      let header = this.$refs['header_' + index][0];
      this.resizerIndex = index;
      this.resizerStartOffset = header.offsetWidth - event.pageX;
    },
    onDragOverResizer(event){
      if (this.resizerStartOffset != null && this.resizerIndex != null) {
        let width = this.resizerStartOffset + event.pageX;
        if (width <= 20) {
          width = 20;
        }
        this.headerWidth[this.resizerIndex] = width;
        this.headerWidth.map((obj, index) => {
          this.$refs['header_' + index][0].width = obj
        })
      }
    },
  },
  props: {
    isHandler : {
      type : Boolean,
      default : true
    },
    tableData: {
      type : Array,
      default(){
        return[
            {
              id: 0,
              name: "sasha",
              age: "27",
              tel: "000-0000-0000",
              address: "address",
              sequence: 0
            }, {
              id: 1,
              name: "sam",
              age: "31",
              tel: "000-0000-0000",
              address: "address",
              sequence: 1
            },
            {
              id: 2,
              name: "rapha",
              age: "26",
              tel: "000-0000-0000",
              sequence: 2
            },
            {
              id: 3,
              name: "dan",
              age: "19",
              tel: "000-0000-0000",
              sequence: 3
            },
            {
              id: 4,
              name: "dean",
              age: "17",
              tel: "000-0000-0000",
              sequence: 4
            }
          ]
      }
    },
    header : {
      type : Array,
      default(){
          return [
          {
            accessor: "sequence",
            Header: "No."
          },
          {
            accessor: "name",
            Header: "NAME"
          }, {
            accessor: "age",
            Header: "AGE"
          }, {
            accessor: "tel",
            Header: "TEL"
          }
        ]
      }
  }
  }
}
</script>

<style scoped>

.resizer_left {
  top: 0;
  bottom: 0;
  width: 10px;
  position: absolute;
  cursor: col-resize;
  left: 0;
}

.resizer_right {
  top: 0;
  bottom: 0;
  width: 10px;
  position: absolute;
  cursor: col-resize;
  right: 0;
}

table {
  border-collapse: collapse;
  table-layout: fixed;
  width : 100%;
}

tr {
  text-align: left;
  border-bottom: 1px solid #ddd;
}

td {
  display: table-cell;
  text-align: left;
  border: 1px solid #ddd;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  max-width: 0;
  min-width: 20px;
}

th {
  background-color: #ccc;
  text-align: left;
  border: 1px solid #ddd;
  position: relative;
  display: table-cell;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>
