
<!----------СКРИПТИК------->
<script>
export default {
  data: () => ({
    formData: {
      name: "",
      surname: "",
      age: null,
      sex: "",
      frameworks: [],
    },
    sexOptions: Object.freeze({
      M: "M",
      F: "F",
    }),
    frameWorksOptions: Object.freeze({
      VUE: "Vue",
      ANGULAR: "Angular",
      SVELTE: "Svelte",
      REACT: "React",
    }),
  }),
  computed: {
    errors() {
      const errors = [];
     // Валидация имени
     if (!this.formData.name) {
        errors.push({ field: "name", message: "Имя является обязательным полем!" });
      } else if (!/^[a-zA-Zа-яА-Я]+$/.test(this.formData.name)) {
          errors.push({ field: "name", message: "Имя должно содержать только буквы" });
      }
      
        // Валидация фамилии
      if (!this.formData.surname) {
        errors.push({ field: "surname", message: "Фамилия является обязательным полем!" });
      }else if (!/^[a-zA-Zа-яА-Я]+$/.test(this.formData.surname)) {
          errors.push({ field: "surname", message: "Фамилия должна содержать только буквы" });
      }


      // Валидация возраста
      if (!this.formData.age && typeof this.formData.age !== "number") {
        errors.push({ field: "age", message: "Возраст является обязательным полем!" });
      } else if (this.formData.age < 14) {
          errors.push({field: "age", message: "Возраст должен быть старше 14 лет!"});
      }

     // Валидация пола
      if (!this.formData.sex) {
        errors.push({ field: "sex", message: "Пол необходимо указать обязательно!" });
      }
        // Валидация фреймворков
      if (this.formData.frameworks.length === 0) {
        errors.push({ field: "frameworks", message: "Выберите хотя бы один фреймворк!" });
      }
      return errors;
    },
  },
  methods: {
        handleSubmit() {
           if (this.errors.length === 0) {
                console.log("Успешная отправка формы!", this.formData);
                this.$refs.mesModal.classList.add('visible')
             }
        },
        hideModal(){
             this.$refs.mesModal.classList.remove('visible')
             this.formData = {
                name: "",
                surname: "",
                age: null,
                sex: "",
                frameworks: [],
              };
        }
      },
};
</script>

<!----------ХТМЛЬКА------->
<template>
  <div class="header">
    <h1>Моя форма</h1>
    <p>Заполните форму пж!</p>
  </div>
<div class="myform">
  <form @submit.prevent="handleSubmit" class="form">
    <div class="form-group">
        <label for="name">Имя:</label>
        <input type="text" id="name" v-model="formData.name" />
       <div v-if="errors.find(err => err.field === 'name')" class="error">{{ errors.find(err => err.field === 'name').message }}</div>
     </div>
    <div class="form-group">
      <label for="surname">Фамилия:</label>
      <input type="text" id="surname" v-model="formData.surname" />
     <div v-if="errors.find(err => err.field === 'surname')" class="error">{{ errors.find(err => err.field === 'surname').message }}</div>
  </div>
  <div class="form-group">
    <label for="age">Возраст:</label>
    <input type="number" id="age" v-model.number="formData.age" />
      <div v-if="errors.find(err => err.field === 'age')" class="error">{{ errors.find(err => err.field === 'age').message }}</div>
  </div>
    <div class="form-group">
      <label>Пол:</label>
      <div v-for="(option, key) in sexOptions" :key="key">
        <input type="radio" :id="key" v-model="formData.sex" :value="option" />
       <label :for="key">{{ option }}</label>
    </div>
      <div v-if="errors.find(err => err.field === 'sex')" class="error">{{ errors.find(err => err.field === 'sex').message }}</div>
  </div>
  <div class="form-group">
    <label>Фреймворки:</label>
   <div v-for="(option, key) in frameWorksOptions" :key="key">
      <input type="checkbox" :id="key + 'fw'" v-model="formData.frameworks" :value="option" />
     <label :for="key + 'fw'">{{ option }}</label>
   </div>
      <div v-if="errors.find(err => err.field === 'frameworks')" class="error">{{ errors.find(err => err.field === 'frameworks').message }}</div>
 </div>
   <button type="submit" :disabled="errors.length > 0">Отправить</button>
</form>
</div>

<div class="mes-modal" id="mes-modal" ref="mesModal">
  <div class="success-message">
    <p>Форма успешно отправлена!</p>
    <p><b>Имя:</b> {{ formData.name }}</p>
    <p><b>Фамилия:</b> {{ formData.surname }}</p>
    <p><b>Возраст:</b> {{ formData.age }}</p>
    <p><b>Пол:</b> {{ formData.sex }}</p>
    <p><b>Фреймворки:</b> {{ formData.frameworks.join(', ') }}</p>
      <button @click="hideModal">Ок</button>
  </div>
</div>
</template>


<!----------СТИЛЁЧКИ------->
<style>
#app {
  font-family: "Montserrat", sans-serif;
  color: #181a48;
  margin-top: 30px;
  font-size: 18px;;
}
.header{
  padding:20px 50px;
  border-radius: 40px;
  text-align:center;
  background-color: #fff9f9;
}
body{
  width:100%;
  display:flex;
  justify-content: center;
  align-items: center;
  background: rgb(238,174,202);
  background: radial-gradient(circle, rgba(238,174,202,1) 0%, rgba(195,105,201,1) 100%);
}
.myform{
  margin-top:30px;
  padding:20px;
  border-radius: 10px;
  background-color: #fff9f9;
}
button{
  color: #fff9f9;
    padding: 10px 20px;
    font-size:20px;
    margin-top:20px;
    border-radius: 30px;
    background-color: #c369c9;
    border: 2px solid #fff9f9;
    transition: all 0.3s ease;
    cursor: pointer;
}
button:not(:disabled):hover{
  background-color: #fff9f9;
  border-color: #c369c9;
  color: #c369c9;
}

.form {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin: 20px auto;
}
.form-group {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
    text-align: left;
}
.form-group label{
    margin-bottom: 5px;
}
.form-group input,
.form-group select {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
}
.error {
  color: red;
  font-size: 0.8em;
  text-align: left;
  margin-top: 3px;
}
button:disabled{
  background-color: #cccccc;
  color: #666666;
  cursor: not-allowed;
}

.mes-modal {
  position: fixed;
  visibility:hidden;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    overflow: hidden;
    overflow-y: auto;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center;
    align-items: center;
    z-index: 99;
}
.mes-modal.visible { 
  visibility: visible; 
}
.success-message {
  background-color: #99db9b;
  color: white;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
  width: 600px;
    max-width: 100%;
}

</style>
