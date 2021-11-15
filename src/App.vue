<template>
<div class="container">
<div class="header">
 <h1 style="text-align:center;">SILAKAN MENGISI FORM DISINI INI</h1>
</div>

<div class="section">
<div class="nav">
  <h3>silahkan isi form disini</h3>
  <div>
    <br>
     <br>
    <form @submit.prevent="add">
        <label for="">Nama Siswa</label>
        <input required placeholder="isi nama anda disini" class="form-control" style="width: 200px;" type="text" v-model="form.nama"/><br>
        <label for="">Judul Buku</label>
        <input required placeholder="isi judul buku" class="form-control" style="width: 200px;" type="text" v-model="form.judul"/> <br>
        <label for="">Tanggal Pinjam</label>
        <input required placeholder="isi tanggal pinjam" class="form-control" style="width: 200px;" type="date" v-model="form.pinjam"/> <br>
        <label for="">Tanggal Pengembalian</label>
        <input required placeholder="isi tanggal pengembalian" class="form-control" style="width: 200px;" type="date" v-model="form.pengembalian"/> <br>
        <br><br>
        <button class="btn btn-dark" type="submit" v-show="!updateSubmit">Tambah Buku</button>
        <button id="button" type="button" v-show="updateSubmit" @click="update(form)">
          Tambah Data</button> <br>
    </form> 
    <br>
  </div>
</div>
<div class="article">
  <h2>TABEL PINJAMAN BUKU</h2>
  <br>
  <br>
  <center>
        <table border='1'> 
            <thead class="thead-dark">
            <tr> 
              <th>No</th> 
              <th>Nama Siswa</th> 
              <th>judul Buku</th> 
              <th>Tanggal Pinjam</th> 
              <th>Tanggal Pengembalian</th> 
              <th>Aksi</th> 
            </tr> 
           </thead>
            <tbody> 
    <tr v-for="(user,index) in users" :key="index"> 
      <th>{{++index}}</th> 
      <th>{{user.nama}}</th> 
      <th>{{user.judul}}</th> 
      <th>{{user.pinjam}}</th> 
      <th>{{user.pengembalian}}</th> 
      <th>{{user.action}}<button class="btn btn-danger" @click="edit(user)">ubah</button>|<button class="btn btn-primary" @click="del(user)">hapus</button>
      </th> 
    </tr> 
      </tbody> 
         </table>
  </center>
</div>
</div>
<div class="footer">
  <h2>Terima kasih atas kepercayaan anda</h2>
</div>
</div>
</template>

<script>
/* eslint-disable */
import axios from "axios";
export default {
  data() {
    return {
      form: {
        id: "",
        nama: "",
        judul: "",
        pinjam: "",
        pengembalian: "",
      },
      users: "",
      updateSubmit: false,
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/users")
        .then((res) => {
          this.users = res.data; //respon dari rest api dimasukan ke users
        })
        .catch((err) => {
          console.log(err);
        });
    },
    add() {
      axios.post("http://localhost:3000/users", this.form).then((res) => {
        this.load();
        this.form.nama = "";
        this.form.judul = "";
        this.form.pinjam = "";
        this.form.pengembalian = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.id = user.id;
      this.form.nama = user.nama;
      this.form.judul = user.judul;
      this.form.pinjam = user.pinjam;
      this.form.pengembalian = user.pengembalian;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/users/" + form.id, {
          nama: this.form.nama,
          judul: this.form.judul,
          pinjam: this.form.pinjam,
          pengembalian: this.form.pengembalian,
        })
        .then((res) => {
          this.load();
          this.form.id = "";
          this.form.nama = "";
          this.form.judul = "";
          this.form.pinjam = "";
          this.form.pengembalian = "";
          this.updateSubmit = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    del(user) {
      axios.delete("http://localhost:3000/users/" + user.id).then((res) => {
        this.load();
        let index = this.users.indexOf(form.nama);
        this.users.splice(index, 1);
      });
    },
  },
};
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Kelly+Slab&display=swap');
  * {
    box-sizing: border-box;
  }
  
   body {
    font-family: 'Kelly Slab';
  }
  
  /* Style the header */
  .header {
    background-color: lightblue;
    padding: 30px;
    text-align: center;
    font-size: 35px;
    color: rgb(49, 49, 49);
    font-family: 'Kelly Slab';
  }
  
  /* Container for flexboxes */
  .section {
    display: -webkit-flex;
    display: flex;
  }
  
  /* Style the navigation menu */
  .nav {
    -webkit-flex: 1;
    -ms-flex: 1;
    flex: 1;
    background: lightblue;
    padding: 20px;
  }
  
  /* Style the list inside the menu */
  .navul {
    list-style-type: none;
    padding: 0;
  }
  
  /* Style the content */
  .article {
    -webkit-flex: 3;
    -ms-flex: 3;
    flex: 3;
    background-color: lightblue;
    padding: 10px;
    text-align: center;
  }
  
  /* Style the footer */
  .footer {
    background-color: lightblue;
    padding: 10px;
    text-align: center;
    color: white;
  }
  
.container {
  margin: 50px;
}
</style>