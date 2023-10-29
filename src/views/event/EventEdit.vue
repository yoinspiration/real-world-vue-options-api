<template>
  <p>Edit the event here</p>
</template>

<script>
export default {
  props: ['event'],
  data() {
    return {
      unSavedChanges: false
    }
  },
  beforeRouteLeave() {
    if (this.unSavedChanges) {
      const answer = window.confirm('Do you really want to leave? You have unsaved change!')
      if (!answer) {
        return false
      }
    }
  }
}
</script>
