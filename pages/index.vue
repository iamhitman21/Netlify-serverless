<template>
  <div class="container">
    <h1>Serverless Contact form</h1>
    <form @submit.prevent="sendContactToLambdaFunction">
      <div>
        <label for="name">Name</label>
        <input id="name" v-model="form.name" type="text">
      </div>
      <div>
        <label for="email">Email</label>
        <input id="email" v-model="form.email" type="email">
      </div>
      <div>
        <label for="">Message</label>
        <input id="message" v-model="form.message" type="text" placeholder="Type your message">
      </div>
      <button>Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      form: {
        name: '',
        email: '',
        message: ''
      }
    }
  },
  methods: {
    async sendContactToLambdaFunction () {
      try {
        const response = await this.$axios.$post('/.netlify/functions/contact-mail', {
          issuerName: this.form.name,
          issuerEmail: this.form.email,
          message: this.form.message
        })
        this.$toast({
          title: 'Mail sent',
          description: response,
          status: 'success',
          duration: 10000,
          isClosable: true
        })
        this.form.name = ''
        this.form.email = ''
        this.form.message = ''
      } catch (error) {
        this.$toast({
          title: 'An error occured',
          description: error,
          status: 'error',
          duration: 10000,
          isClosable: true
        })
      }
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
