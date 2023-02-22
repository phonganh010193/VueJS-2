<template>
  <div class="container">
    <form class="form-content">
      <div class="row">
        <div class="col">
          <label for="">FirstName</label>
          <input 
            type="text" 
            class="form-control"
            :value="firstName"
            @input="(e) => firstName = e.target.value"
          >
        </div>
        <div class="col">
          <label for="">LastName</label>
          <input 
            type="text" 
            class="form-control" 
            v-model="lastName"
          >
        </div>
      </div>
      <div class="form-group">
        <label for="">Address</label>
        <input 
          class="form-control"
          v-model="address"
        >
      </div>
      <div class="form-group">
        <label for="">Age</label>
        <input 
          class="form-control"
          v-model="age"
        >
      </div>
      <div class="form-group">
        <label for="">PhoneNumber</label>
        <input 
          class="form-control"
          v-model="phoneNumber"
        >
      </div>
      <div class="form-check">
        <input class="form-check-input" type="checkbox" v-model="published">
        <label class="form-check-label" for="">
          Check me out
        </label>
      </div>
      <button 
        class="btn btn-primary"
        @click.prevent="createUser"
      >Create User</button>
    </form>
    <table 
      :class="[`table table-${theme} table-sm text-center `]"
    >
      <thead>
        <tr>
          <th>Name</th>
          <th>Adress</th>
          <th>Age</th>
          <th>PhoneNumber</th>
        </tr>
      </thead>
      <tbody v-for="(item, index) in listPagesByPublished" v-if="pages.length > 0" :key="index">
        <tr>
          <th>{{ fullName(item) }}</th>
          <td>{{ item.info.address }}</td>
          <td>{{ item.info.age }} age</td>
          <td>{{ item.info.phoneNumber }}</td>
        </tr>
      </tbody>
    </table>
    <div>
      <button
        class="btn btn-primary"
        @click.prevent="changeThemeTable(table.border)"
      >TableBorder</button>
      <button
        class="btn btn-primary"
        @click.prevent="changeThemeTable(table.striped)"
      >TableStriped</button>
      <button 
        class="btn btn-primary"
        @click.prevent="changeThemeTable(table.dark)"
      >TableDark</button>
      <button 
        class="btn btn-primary"
        @click.prevent="changeThemeTable(table.hover)"
      >TableHover</button>
    </div>
  </div>
</template>

<script>


export default {
  created() {
    this.getPages()
  },
  data() {
    return {
      firstName:'',
      lastName: '',
      address: '',
      age: '',
      phoneNumber: '',
      published: true,
      theme: 'dark',
      table: {
        border: 'bordered',
        dark: 'dark',
        striped: 'striped',
        hover: 'hover'
      },
      pages:[]
    }
  },
  computed:{ 
    listPagesByPublished() {
      return this.pages.filter(el => el.published)
    }
  },
  methods: {
    fullName(item) {
      return item.firstName + ' ' + item.lastName;
    },
    changeThemeTable(theme) {
      console.log('theme', theme)
      this.theme = theme;
    },
    async getPages() {
      let res = await fetch('pages.json')
      let data = await res.json();
      this.pages = data
    },
    createUser() {
      const value = {
        firstName: this.firstName,
        lastName: this.lastName,
        info: {
          address:this.address,
          age:this.age,
          phoneNumber: this.phoneNumber
        },
        published: this.published
      }
      console.log(value)
      this.pages.push(value)
      this.firstName = '';
      this.lastName = '';
      this.address = '';
      this.age = '';
      this.phoneNumber = '';
    }
  }
  
}
</script>

<style>
  .form-content {
    width: 400px;
    padding-bottom: 10px;
    margin: 0 auto;
  }

</style>



