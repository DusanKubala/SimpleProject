<template>
  <form @submit.prevent="handleSubmit">
      <label>Title:</label>
      <input type="text" v-model="title" required>
      <label>Details</label>
      <textarea v-model="details" required></textarea>

      <button>Update project</button>
  </form>
</template>

<script>
export default {
    props: [ 'id' ],
    data() {
        return {
            title: '',
            details: '',
            uri: 'http://localhost:3000/projects/' + this.id
        }
    },
    mounted() {
        fetch(this.uri)
            .then(res => res.json())
            .then(data => {
                this.title = data.title
                this.details = data.details
            })
    },
    methods: {
        handleSubmit() {
            fetch(this.uri, {
                method: 'PATCH',
                headers: { 'Content-type': 'application/json' },
                body: JSON.stringify({ title: this.title, details: this.details }) 
            }).then(() => {
                this.$router.push('/')
            }).catch((err) => console.log(err))
        }
    }
    
}
</script>

<style>
    form {
        background: white;
        padding: 20px;
        border-radius: 10px;
    }

    label {
        display: block;
        color: #bbb;
        text-transform: uppercase;
        font-size: 14px;
        font-weight: bold;
        letter-spacing: 1px;
        margin: 20px 0 10px 0;
    }

    input {
        padding: 10px;
        border: 0;
        border-bottom: 1px solid #ddd;
        width: 100%;
        box-sizing: border-box;
    }

    textarea {
        padding: 10px;
        height: 100px;
        border: 1px solid #ddd;
        width: 100%;
        box-sizing: border-box;
        
    }

    form button {
        display: block;
        color: black;
        margin: 35px 0;
        width: 100%;
        height: 35px;
        font-size: 1.5em;
        border: none;
        border-radius: 10px;
        background: rgb(121, 203, 214);
        cursor: pointer;
    }

    form button:hover {
        background:rgb(175, 233, 241);
    }
</style>