<template>
  <table>
    <tr>
      <th><input ref="checkAll" @change="toggleSelectAll" type="checkbox" /></th>
      <th>Name</th>
      <th>Contact</th>
      <th>Country</th>
    </tr>
    <tr>
      <td colspan="4">TableSet: count {{ selection.size }}</td>
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
  name: 'TableSet',
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selection: new Set(),
    }
  },
  methods: {
    everyItem() {
      return this.items.length === this.selection.size
    },
    someItems() {
      return !this.everyItem() && !!this.selection.size
    },
    toggleSelectAll(e) {
      console.time('toggleSelectAll')
      const value = e.target.checked
      if (value) {
        this.selection = new Set(this.items)
      } else {
        this.selection = new Set()
      }
      console.timeEnd('toggleSelectAll')
    },
    isSelected(item) {
      return !!this.selection.has(item) || false
    },
    select(item, value = true) {
      console.time('select')
      if (value) {
        this.selection.add(item)
      } else {
        this.selection.delete(item)
      }
      this.selection = new Set(this.selection)

      if (this.everyItem()) {
        this.$refs.checkAll.indeterminate = false
        this.$refs.checkAll.checked = true
      } else if (!this.everyItem() && this.someItems()) {
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