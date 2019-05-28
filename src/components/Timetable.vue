<template>
  <div>
    <div class="block" @dblclick="editText">
      <input class="editText" v-model="text" @input="submitText" hidden />
      <span class="text">{{text}}</span>
      <div class="colorSelectContainer">
        <select class="colorSelect">
          <option selected>White</option>
          <option>Red</option>
          <option>Orange</option>
          <option>Yellow</option>
          <option>Green</option>
          <option>Blue</option>
          <option>Indigo</option>
          <option>Violet</option>
        </select>
      </div>
      <div class="dragPoint" @mousedown="dragStart"></div>
    </div>
    <div class="block">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Timetable',
  props: {
    msg: String
  },
  data: () => {
    return {
      text: "hi",
      draging: false,
      target: null,
      originalX: 0,
      originalY: 0,
      originalHeight: 0,
      originalWidth: 0,

      editing: null,
    }
  },
  mounted: function() {
    window.addEventListener('mouseup', this.draggEnd);
  },
  methods: {
    editText: function(e){
      console.log(e);
      var editText = e.target.getElementsByClassName('editText')[0];
      var text = e.target.getElementsByClassName('text')[0];
      this.editing = e.target;
      editText.hidden = false;
      text.hidden = true;
    },
    submitText: function(e){
      console.log(e);
      var editText = this.editing.getElementsByClassName('editText')[0];
      var text = this.editing.getElementsByClassName('text')[0];
      this.editing = null;
      editText.hidden = true;
      text.hidden = false
    },
    drag: function(e){
      if(this.draging){
        var currentX = e.x;
        var currentY = e.y;
        this.target.style.width = (this.originalWidth + currentX - this.originalX) + "px";
        this.target.style.height = (this.originalHeight + currentY - this.originalY) + "px";
        console.log(this.target);
      }
    },
    draggEnd: function(e){
      this.draging = false;
      this.target = null;
      window.removeEventListener('mousemove', this.drag);
    },
    dragStart: function(e){
      this.draging = true;
      this.target = e.srcElement.parentNode;
      this.originalX = e.x;
      this.originalY = e.y;
      this.originalWidth = this.target.offsetWidth;
      this.originalHeight = this.target.offsetHeight;
      window.addEventListener('mousemove', this.drag);
    },
  }
}

</script>


<style scoped>
  #main {
    background-color: BurlyWood;
    float: right;
    position: absolute;
    height: 200px;
    right: 0;
    left: 200px;
    margin-top: 10px;
  }

  .block{
    background-color: IndianRed;
    width:200px;
    height:200px;
    border: 2px solid black;
    display: table-cell;
    margin-top: -2px;
    margin-left: -2px;
    position: relative;
    box-sizing: content-box;
    text-align: center;
    vertical-align: middle;

  }

  .editText{
    text-align: center;
  }

  .dragPoint {
    width: 10px;
    height: 10px;
    border-radius: 50%; 
    background: white;
    border: 3px solid #4286f4;
    right: -5px;
    bottom: -5px;
    position: absolute;
    z-index: 20;
    cursor: se-resize;
  }

  .colorSelectContainer {
    right: 3px;
    top: 3px;
    position: absolute;
    opacity: 0;
  }

  .colorSelectContainer:hover{
    opacity: 1;
  }

  .colorSelect{
    background-color: white;
  }
</style>