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
           <div style="background-color: #ddd; color: #777; height: 120px; width: 300px">
             Testando
             <h1>Fala memo</h1>
           </div>
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
    createJSONElement (el) {
      const obj = {}
      // if (!!el.innerHTML) obj.content = el.innerHTML
      if (!!el.textContent) obj.content = el.textContent
      if (!!el.style) {
        obj.style = {}
        for (const attr in el.style) {
          obj.style[attr] = el.style[attr]
        }
      }
      if (!!el.childNodes && !!el.childNodes.length) {
        const toArray = Array.from(el.childNodes)
        obj.children = []
        for (const child in toArray) {
          const c = this.createJSONElement(child)
          obj.children.push(c)
        }
      }
      return obj
    },
    mountJSON (nodes) {
      let json = []
      for (const child of nodes) {
        const node = this.createJSONElement(child)
        json.push(node)
      }
      return json
    }
  },
  mounted () {
    const box = this.$refs['edit']
    const toArray = Array.from(box.childNodes)
    const json = this.mountJSON(toArray)
    console.log(json)
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
        outline: none;
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
