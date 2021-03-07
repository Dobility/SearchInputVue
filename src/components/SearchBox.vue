<template>
  <form class="input" ref="Form" @submit="onSubmit">
    <div class="input-area">
      <input
          ref="Input"
          v-model="input"
          autofocus
          @mousedown="clickInput = true"
          @focus="showHint = true"
          @blur="showHint = false"
          @keydown.up.prevent="onSelect($event, -1)"
          @keydown.down="onSelect($event, 1)"
          @keydown.enter="onConfirm"
          @input="toggleHint"
      />
      <label v-if="input" @mousedown="input = ''; focusInput()">×</label>
      <button>搜索</button>
    </div>
    <div class="input-hint" v-show="showHint && clickInput">
      <ul class="input-hint-box" v-show="showHis">
        <li
            v-for="(sug, i) in hisList"
            v-html="sug"
            :key="i"
            :class="{ active: i === selectIndex }"
            @mousedown="input = sug"
        />
      </ul>
      <ul class="input-hint-box" v-show="showSug">
        <li
            v-for="(sug, i) in sugList"
            v-html="sug"
            :key="i"
            :class="{ active: i === selectIndex }"
            @mousedown="input = sug; showHint = false"
        />
      </ul>
    </div>
  </form>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      hisList: [
          'hello',
          'hello world',
          'hello mom',
          'hello hi',
          'hello bye'
      ],
      clickInput: false,
      showHint: false,
      showHis: true,
      showSug: false,
      selectIndex: -1,
      selectInput: '',
      realInput: '',
    }
  },
  computed: {
    input: {
      get() {
        return this.selectInput || this.realInput
      },
      set(val) {
        this.selectInput = ''
        this.realInput = val
        this.selectIndex = -1
        this.showHint = true
        this.clickInput = true
        this.showSug = !!val
        this.showHis = !val
      }
    },
    sugList() {
      const s = val => `${this.realInput}${val}`;
      return [
          s('123'),
          s('456'),
          s('789'),
          s('11111'),
          s('00000')
      ]
    },
  },
  methods: {
    onSelect(keyEvent, dir) {
      if (!keyEvent.ctrlKey && !keyEvent.shiftKey && !keyEvent.altKey && !keyEvent.metaKey) {
        if (this.clickInput && this.showHint) {
          let next = this.selectIndex + dir
          if (next < 0) {
            next = this.sugList.length;
          } else if (next >= this.sugList.length) {
            next = -1;
          }
          this.selectIndex = next
          const list = this.showHis ? this.hisList : this.sugList
          this.selectInput = list[next] || ''
        } else if (dir > 0) {
          this.clickInput = true
          this.showHint = true
        }
      }
    },
    onConfirm(e) {
      if (this.showHint && (this.showHis || this.showSug) && this.selectInput) {
        e.preventDefault()
        this.input = this.selectInput
        this.showHint = false
      }
    },
    onSubmit(e) {
      if (!this.input) {
        e.preventDefault()
        this.focusInput()
      }
    },
    focusInput() {
      this.$nextTick(() => {
        this.$refs.Input.focus()
      })
    },
    toggleHint(e) {
      this.input = e.target.value
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@color: #666;
.input {
  width: 400px;
  height: 30px;
  margin: auto;
  position: relative;
  border: 1px solid @color;
  &-area {
    display: flex;
    height: 100%;
    input {
      padding: 0 5px;
      flex: 1;
      font-size: inherit;
      border: none;
      outline: none;
    }
    label {
      align-self: center;
      padding: 5px;
    }
    button {
      width: 100px;
      border: none;
      background: @color;
      color: white;
      margin: -1px;
      outline: none;
    }
  }
  &-hint {
    position: absolute;
    left: -1px;
    right: -1px;
    text-align: left;
    background: white;
    ul {
      padding: 0;
      margin: 0;
      border: 1px solid gray;
    }
    li {
      list-style: none;
      padding: 6px;
      font-size: 15px;
      &.active,
      &:hover {
        background: lightgray;
      }
    }
  }
}
</style>
