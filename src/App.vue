<template>
  
  <div class="bungkus">

    <h1><b>CRUD</b></h1><br>

    <form @submit.prevent="save">
      <div class="form-group">
        <input type="hidden" v-model="form.id" class="form-control" placeholder="Masukan Data">
        <input type="text" v-model="form.name" class="form-control" placeholder="Masukan Data">
      </div>
      <button type="submit" v-show="!updateSubmit" name="button" class="btn btn-primary">Simpan</button>
      <button type="button" v-show="updateSubmit" v-on:click="update(form)" name="button" class="btn btn-primary">Update</button>
    </form><br>

    <!-- <form @submit.prevent="save">
      <input type="hidden" v-model="form.id" name="" value="">
      <input type="text" v-model="form.name" name="" value=""><br>
      <button type="submit" v-show="!updateSubmit" name="button">Simpan</button>
      <button type="button" v-show="updateSubmit" v-on:click="update(form)" name="button">Update</button>
    </form> -->
  
    <h4>List Users</h4>

    <table class="table" v-for="user in users" :key="user.id">
      <tbody>
        <tr>
          <th scope="row">{{ user.id }}</th>
          <td>{{ user.name }}</td>
          <td><button class="btn btn-warning" type="button" v-on:click="edit(user)" name="button" style="position: absolute;">Edit</button></td>
          <td><button class="btn btn-danger" type="button" v-on:click="hapus(user)" name="button" style="position: absolute;">Hapus</button></td>
        </tr>
      </tbody>
    </table>

    <!-- <ul v-for="user in users" :key="user.id">
      <li>{{ user.name }}
        <button type="button" v-on:click="edit(user)" name="button">Edit</button>
        <button type="button" v-on:click="hapus(user)" name="button">Hapus</button>
      </li>
    </ul> -->

  </div>

</template>

<script>

import axios from 'axios'

export default {
  data(){
    return{
      form:{
        id: '',
        name: ''
      },
      users: [],
      updateSubmit: false
    }
  },
  methods:{
    load(){
      axios.get('http://localhost:3000/users')
      .then((res)=>{
        this.users = res.data
      })
      .catch(()=>{
        alert('error load data')
      })
    },
    save(){
      axios.post('http://localhost:3000/users', this.form)
      .then(()=>{
        this.load()
        this.form.name = ''
        alert('menyimpan...')
      })
      .catch(()=>{
        alert('gagal menyimpan')
      })
    },
    edit(user){
      this.updateSubmit = true
      this.form.id = user.id
      this.form.name = user.name
    },
    hapus(user){
      axios.delete('http://localhost:3000/users/' + user.id)
      .then(()=>{
        this.load()
        this.form.name = ''
        alert('deleted...')
      })
      .catch(()=>{
        alert('error delete')
      })
    },
    update(form){
      axios.put('http://localhost:3000/users/' + form.id, {
        name: this.form.name
      })
      .then(()=>{
        this.load()
        this.form.name = ''
        this.updateSubmit = false
        alert('updated...')
      })
      .catch(()=>{
        alert('update error')
      })
    }
  },
  mounted(){
    this.load()
  }
}
</script>

<style lang="css" scoped>

.bungkus{
  margin-top: 3%;
  margin-left: 25%;
}
.form-control{
  width: 70%;
}
.table{
  width: 70%;
}
.button{
  margin: 5%;
  align-content: center;
}
.button2{
  margin: 5%;
  align-content: center;
}
  
</style>
