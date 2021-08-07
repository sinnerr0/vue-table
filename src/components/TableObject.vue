<template>
  <table>
    <tr>
      <th><input ref="checkAll" @change="toggleSelectAll" type="checkbox" /></th>
      <th>Name</th>
      <th>Contact</th>
      <th>Country</th>
    </tr>
    <tr>
      <td colspan="4">TableObject: count {{ Object.keys(selection).length }}</td>
    </tr>
    <tr v-for="item in items" :key="item.id">
      <td><input type="checkbox" @change="select(item, $event.target.checked)" :checked="isSelected(item)" /></td>
      <td>{{ item.name }}</td>
      <td>{{ item.phone }}</td>
      <td>{{ item.country }}</td>
    </tr>
  </table>
</template>

<script>
export default {
  name: 'TableObject',
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selection: {},
    }
  },
  computed: {
    everyItem() {
      return this.items.length && this.items.every((item) => item.isSelected)
    },
    someItems() {
      return this.items.some((item) => item.isSelected)
    },
  },
  methods: {
    toggleSelectAll(e) {
      console.time('toggleSelectAll')
      const value = e.target.checked
      const selection = Object.assign({}, this.selection)

      for (let i = 0; i < this.items.length; i++) {
        const item = this.items[i]
        const key = item.id
        if (value) {
          selection[key] = item
          item.isSelected = true
        } else {
          delete selection[key]
          item.isSelected = false
        }
      }

      this.selection = selection
      console.timeEnd('toggleSelectAll')
    },
    isSelected(item) {
      return !!this.selection[item.id] || false
    },
    select(item, value = true) {
      console.time('select')
      const selection = Object.assign({}, this.selection)
      const key = item.id
      if (value) {
        selection[key] = item
        item.isSelected = true
      } else {
        delete selection[key]
        item.isSelected = false
      }
      this.selection = selection

      if (this.everyItem) {
        this.$refs.checkAll.indeterminate = false
        this.$refs.checkAll.checked = true
      } else if (!this.everyItem && this.someItems) {
        this.$refs.checkAll.indeterminate = true
      } else {
        this.$refs.checkAll.indeterminate = false
        this.$refs.checkAll.checked = false
      }
      console.timeEnd('select')
    },
  },
}
</script>