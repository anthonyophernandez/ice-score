<template>
  <div class="w-full xl:w-1/4 lg:w-1/3 md:w-1/2 sm:w-full bg-white border rounded-lg overflow-hidden p-6">
    <h2 class="text-4xl text-gray-500 font-semibold text-center uppercase tracking-wide">Create Task</h2>
    <Form :task="task" :handleSubmit="createTask" buttonText="Add" :error="error"/>
  </div>
</template>

<script>
export default {
  name: 'CreateTask',
  components: {
    Form: () => import(/* webpackChunkName: 'Form' */ '../components/Form.vue')
  },
  data () {
    return {
      error: '',
      task: {
        description: '',
        impact: 5,
        confidence: 5,
        ease: 5,
        iceScore: 5
      }
    }
  },
  methods: {
    async createTask () {
      if (this.task.description) {
        const iceScore = (parseInt(this.task.impact) + parseInt(this.task.confidence) + parseInt(this.task.ease)) / 3
        this.task.iceScore = Number.isInteger(iceScore) ? iceScore : iceScore.toFixed(2)
        await this.$store.dispatch('tasks/create', this.task)
        this.$router.push({ name: 'Home' })
      } else {
        this.error = 'You must introduce a description'
      }
    }
  }
}
</script>

<style>

</style>
