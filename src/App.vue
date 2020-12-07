<template>
  <div>
    <ul v-for="user in users" :key="user.id">
      <li>
        <span>{{user.name}}</span> &#160;
        <span>{{user.tempat_tanggal_lahir}}</span> &#160;
        <span>{{user.Sekolah}}</span> &#160;
        <span>{{user.Jurusan}}</span> &#160;
        <span>{{user.Kelas}}</span> &#160;
        <button @click="edit(user)">Edit</button> || <button @click="del(user)">Delete</button>
      </li>
    </ul>
  </div>
  <form @submit.prevent="add">
    <input type="hidden" v-model="form.id">
    <input type="text" v-model="form.name">
    <input type="text" v-model="form.tempat_tanggal_lahir">
    <input type="text" v-model="form.Sekolah">
    <input type="text" v-model="form.Jurusan">
    <input type="text" v-model="form.Kelas">
    <button type="submit" v-show="!updateSubmit">add</button>
    <button type="button" v-show="updateSubmit" @click="update(form)">update</button>
  </form>
</template>

<script>
/* eslint-disable */
import axios from 'axios'
export default {
  data(){
    return{
     form: {
       id: '',
       name: '',
       tempat_tanggal_lahir: '',
       Sekolah: '',
       Jurusan: '',
       Kelas: '',
     },
     users: '',
     updateSubmit: false
    }
  },
  mounted(){
    this.load()
  },
  methods: {
    load(){
      axios.get('http://localhost:3000/user').then(res =>{
        this.users = res.data
      }).catch ((err) => {
        console.log(err);
      })
    },
    add(){
      axios.post('http://localhost:3000/user/', this.form).then(res =>{
        this.load()
        this.form.name = ""
        this.form.tempat_tanggal_lahir = ""
        this.form.Sekolah = ""
        this.form.Jurusan = ""
        this.form.Kelas = ""
      })
    },
      edit(user){
      this.updateSubmit = true
      this.form.id = user.id
      this.form.name = user.name
      this.form.tempat_tanggal_lahir = user.tempat_tanggal_lahir
      this.form.Sekolah = user.Sekolah
      this.form.Jurusan = user.Jurusan
      this.form.Kelas = user.Kelas
    },
    update(form){
      return axios.put('http://localhost:3000/user/' + form.id, {name: this.form.name, tempat_tanggal_lahir: this.form.tempat_tanggal_lahir, Sekolah: this.form.Sekolah, Jurusan: this.form.Jurusan, Kelas: this.form.Kelas} ).then (res =>{
        this.load()
        this.form.id = ''
        this.form.name = ''
        this.form.tempat_tanggal_lahir = ''
        this.form.Sekolah = ''
        this.form.Jurusan = ''
        this.form.Kelas = ''
        this.updateSubmit = false
      }).catch ((err) => {
        console.log(err);
    })
    },
    del(user){
      axios.delete('http://localhost:3000/user/'+ user.id).then(res =>{
        this.load()
        let index = this.user.indexOf(form.name)
        this.users.splice(index,1)
      }) 
    }
  }
}
</script>
