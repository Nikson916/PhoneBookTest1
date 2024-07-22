<script>
export default{
  data(){
    return{
      users: [{userID: "1", userLName: "Иванов", userFName: "Иван", userSName: "Иванович", userNumber:"+7(999)999-99-99"},
              {userID: "2", userLName: "Жуков", userFName: "Алексей", userSName: "Андреевич", userNumber:"+7(999)999-99-98"},
              {userID: "3", userLName: "Белкин", userFName: "Андрей", userSName: "Алексеевич", userNumber:"+7(999)999-99-97"},
              {userID: "4", userLName: "Медведев", userFName: "Максим", userSName: "Васильевич", userNumber:"+7(999)999-99-96"}
              ],
      error: "",
      errorDup: "",
      userNumber: "",
      userID: "5",
      userFName: "",
      userLName: "",
      userSName: "",
      isDuplicate: false,
      maxLenghtText: 50,
      maxLenghtNumber: 20          

    }
  },

  mounted() {
    const savedData = localStorage.getItem("users");
    if (savedData) {
      this.users=JSON.parse(savedData);
    }
    
  },
  methods: {

    filterNumber() {
      this.userID = this.userID.replace(/[^0-9]/g,"");
      if (this.userID.length>this.maxLenghtNumber) {
      this.userID = this.userID.substring(0,this.maxLenghtNumber);
      }     
    },

    filterLName() {
    this.userLName = this.userLName.replace(/[^a-zа-я]/gi,"");
      if (this.userLName.length>this.maxLenghtText) {
      this.userLName = this.userLName.substring(0,this.maxLenghtText);
      }
    },

    filterFName() {
      this.userFName = this.userFName.replace(/[^a-zа-я]/gi,"");
      if (this.userFName.length>this.maxLenghtText) {
      this.userFName = this.userFName.substring(0,this.maxLenghtText);
      }
    },    

    filterSName() {
      this.userSName = this.userSName.replace(/[^a-zа-я]/gi,"");
      if (this.userSName.length>this.maxLenghtText) {
      this.userSName = this.userSName.substring(0,this.maxLenghtText);
      }
    },

    sendData() {

       if(this.userLName == "") {
        this.error="Незаполнено поле Фамилия"
        return;
      } else if(this.userFName == "") {
        this.error="Незаполнено поле Имя"
        return;
      }

      this.error = "";

      if(this.users.some(recordID => recordID.userID === this.userID)) {
        this.isDuplicate = true
        this.errorDup="Ошибка: ID уже занят";
        return;
      } else {
        this.users.push({userID: this.userID, userLName: this.userLName, userFName: this.userFName, userSName: this.userSName});
        this.isDuplicate = false;
        localStorage.setItem("users", JSON.stringify(this.users));
      }
      
      this.errorDup= "";

      this.userID ++;
      this.userFName = "";
      this.userLName = "";
      this.userSName = "";

    },

    filterNumberEdit(index) {
      this.users[index].userNumber = this.users[index].userNumber.replace(/[^-+()0-9]/g,"");
      if (this.users[index].userNumber.length>this.maxLenghtNumber) {
      this.users[index].userNumber = this.users[index].userNumber.substring(0,this.maxLenghtNumber);
      }                 
    },

    filterLNameEdit(index) {
      this.users[index].userLName = this.users[index].userLName.replace(/[^a-zа-я]/gi,"");
      if (this.users[index].userLName.length>this.maxLenghtText) {
      this.users[index].userLName = this.users[index].userLName.substring(0,this.maxLenghtText);
      }           
    },

    filterFNameEdit(index) {
      this.users[index].userFName = this.users[index].userFName.replace(/[^a-zа-я]/gi,"");
      if (this.users[index].userFName.length>this.maxLenghtText) {
      this.users[index].userFName = this.users[index].userFName.substring(0,this.maxLenghtText);
      }           
    },

    filterSNameEdit(index) {
      this.users[index].userSName = this.users[index].userSName.replace(/[^a-zа-я]/gi,"");
      if (this.users[index].userSName.length>this.maxLenghtText) {
      this.users[index].userSName = this.users[index].userSName.substring(0,this.maxLenghtText);
      }           
    },

    DeleteData(index) {
      this.users.splice(index, 1);
      localStorage.setItem("users", JSON.stringify(this.users));     

    },

    EditData(index) {
      this.users[index].editing = true;
      
    },

    stopEditData(index) {
      this.users[index].editing = false;
      localStorage.setItem("users", JSON.stringify(this.users));

    },

    DeletePhoneNumber(index) {
      delete this.users[index].userNumber;
      localStorage.setItem("users", JSON.stringify(this.users));

    }       
  
  }
  
}
</script>

<template>

<br>
<label for="lastname"> Введите ID: </label>
<input id="ID" v-model="userID" @input="filterNumber" placeholder=""> <br> <br>
<label for="lastname"> Введите фамилию: </label>
<input id="lastname" v-model="userLName" @input="filterLName" placeholder="Иванов"> <br> <br>
<label for="firstname"> Введите имя: </label>
<input  id="firstname" v-model="userFName" @input="filterFName" placeholder="Иван"> <br> <br>
<label for="surname"> Введите отчетсво: </label>
<input  id="surname" v-model="userSName" @input="filterSName" placeholder="Иван"> <br> <br>

<p className="errorDup"> {{ errorDup }}</p>
<p className="error"> {{ error }}</p>

<p><button @click="sendData()"> Сохранить пользователя </button></p> <br>

<div v-if="users.length == 0">
<h4>Пользователи не созданы</h4>
</div>

<div v-else>
<p> Пользователей существует {{ users.length }}</p>
</div>

<div>
  <div v-for="(el, index) in users" :key="index">
    <div v-if="!el.editing">
  <p>ID: {{ el.userID }} Фамилия: {{ el.userLName }} - Имя: {{ el.userFName }} - Отчество: {{ el.userSName }} - Номер телефона: {{ el.userNumber }}
    <button @click="EditData(index)"> Редактировать/Добавить номер телефона </button>
    <button @click="DeleteData(index)"> Удалить </button>
    <button @click="DeletePhoneNumber(index)"> Удалить номер телефона </button>
  </p>
  </div>
  <div v-else>
      <input v-model="el.userID" readonly>
      <input v-model="el.userLName" @input="filterLNameEdit(index)">
      <input v-model="el.userFName" @input="filterFNameEdit(index)">
      <input v-model="el.userSName" @input="filterSNameEdit(index)">
      <input v-model="el.userNumber" @input="filterNumberEdit(index)">
      <button @click="stopEditData(index)"> Сохранить </button>
    </div>
  </div>
</div>
  
</template>

<style scoped>

</style>
