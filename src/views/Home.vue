<template>
  <div class="joberson-mount">
    <div class="joberson-mount__options">
      <button name="edit" @click="edit = !edit">{{ edit ? 'Preview' : 'Edit' }}</button>
    </div>
    <div class="joberson-mount__box">
      <div ref="edit"
           class="joberson-mount__box__editor"
           :class="{
             edit: edit
           }"
           :contenteditable="edit">
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'joberson-mount',
  data () {
    return {
      edit: true
    }
  },
  methods: {
    createElement (data) {
      let element = document.createElement(data.el)
      if (!!data.style) {
        for (const attr in data.style) {
          element.style[attr] = data.style[attr]
        }
      }
      if (!!data.content) element.innerHTML = element.innerHTML + data.content
      if (!!data.children && !!data.children.length) {
        for (const child of data.children) {
          const c = this.createElement(child)
          element.appendChild(c)
          console.log('Created and appended to element ', child.el)
        }
      }
      console.log('Returned ', data.el)
      return element
    },
    mountContent (elements) {
      const box = this.$refs['edit']
      for (const el of elements) {
        const element = this.createElement(el)
        box.appendChild(element)
        console.log('Created and appended ', el.el)
      }
    }
  },
  mounted () {
    this.mountContent([{
      el: 'div',
      style: {
        'height': '150px',
        'width': '400px',
        'top': '10px',
        'left': '8px',
        'font-size': '22px',
      },
      content: 'joberson-mount',
      children: [
        {
          el: 'div',
          style: {
            'background-color': '#000',
            'color': '#fff',
            'height': '40px',
            'width': '40px'
          }
        },
        {
          el: 'h1',
          content: 'LOREN IPSUN DOLOR'
        }
      ]
    }])
  }
}
</script>

<style lang="scss">
  .joberson-mount {
    display: flex;
    justify-content: center;
    align-items: stretch;
    flex-direction: column;
    width: 100%;
    height: calc(100% - #{78px});
    &__options {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-basis: 100px;
      button {
        margin: 3px 8px;
        padding: 4px 16px;
        border: none;
        border-radius: 25px;
        outline: none
      }
    }
    &__box {
      display: flex;
      flex-grow: 1;
      padding: 20px;
      &__editor {
        position: relative;
        flex-grow: 1;
        border: 2px solid #55b883;
        &.edit {
          div {
            border: 2px dotted #000;
          }
        }
      }
    }
  }
</style>
