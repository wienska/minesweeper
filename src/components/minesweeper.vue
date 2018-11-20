<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div class="status">{{ status }}</div>
    <div v-for="(row, rowIndex) in field" :key="rowIndex" class="field_row">
      <div
        v-for="(item, index) in row"
        :key="index"
        class="field_item"
        :class="{ __flag: item.flag }"
        @click.prevent="handleClick(item, rowIndex, index)"
        @contextmenu.prevent="setFlag(item)">
          <span v-show="item.show">{{ item.value }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      field: [],
      rows: 10,
      cols: 10,
      minesNumber: 10,
      mines: [],
      status: ''
    }
  },
  mounted () {
    this.getMines();
    this.createField();
    this.fillField();
  },
  methods: {
    getMines () {
      for (let k = 0; k < this.minesNumber; k++) {
        this.mines.push([Math.floor(Math.random() * 10), Math.floor(Math.random() * 10)])
      }
    },
    createField () {
      for (let i = 0; i < this.rows; i++) {
        this.field.push([]);
        for(let j = 0; j < this.cols; j++) {
          this.field[i].push({ show: false, value: 0, flag: false });
        }
      }
    },
    fillNumbers (item, row) {
      if (this.field[row][item[1]].value !== 'x') this.field[row][item[1]].value++;
      if (item[1] !== 0 && this.field[row][item[1] - 1].value !== 'x') this.field[row][item[1] - 1].value++;
      if (item[1] !== this.cols - 1 && this.field[row][item[1] + 1].value !== 'x') this.field[row][item[1] + 1].value++;
    },
    fillField () {
      this.mines.forEach(item => {
        this.field[item[0]][item[1]].value = 'x';
        this.fillNumbers(item, item[0]);
        if (this.field[item[0] - 1] ) this.fillNumbers(item, item[0] - 1);
        if (this.field[item[0] + 1] ) this.fillNumbers(item, item[0] + 1);
      })
    },
    handleClick (item, row, index) {
      if (!item.flag) {
        item.show = true;
        if (item.value === 'x') {
          this.status = 'loose';
        }
        if (!item.value) {
          this.checkPrev(row, index);
          this.checkNext(row, index);
        }
      }
    },
    setFlag (item) {
      item.flag = true;
    },
    checkPrev (row, index) {
      this.field[row][index].show = true;
      if (!this.field[row][index].value) {
        if (index > 0) {
          this.checkPrev(row, index - 1);
        }
      }
    },
    checkNext (row, index) {
      this.field[row][index].show = true;
      if (!this.field[row][index].value) {
        if (index < this.cols - 1) {
          this.checkNext(row, index + 1);
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.field_item {
  width: 26px;
  height: 26px;
  font-size: 20px;
  text-align: center;
  border: 1px solid #999;
  display: inline-block;
  vertical-align: top;
}
.field_item.__flag {
  border: 1px solid red;
}
.status {
  font-weight: bold;
  color: red;
  font-size: 20px;
}
</style>
