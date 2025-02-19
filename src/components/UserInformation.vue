<template>
    <div>
      <h2>User Information</h2>
      <p>Name: {{ name }}</p>
      <p>Address: {{ address }}</p>
      <label for="dob">Enter Date of Birth:</label>
      <input type="date" v-model="dob" id="dob" />
      <button @click="calculateAge">Calculate Age</button>
      <p v-if="age !== null">Age: {{ age }}</p>
      
      <p v-if="age !== null && age < 18" :class="{ error: age < 18 }">You are under age</p>
      <p v-if="age !== null && age >= 18" :class="{ noerror: age >=18 }">You are OK to use the website</p>

      
      <h2>Users List</h2>
      <ul>
        <li v-for="user in users" :key="user.Name">
          <p>UserName: {{ user.Name }}, Age: {{ user.Age }}</p>
          <img :src="user.image" alt="User Image" width="100" />
        </li>
      </ul>
    </div>
  </template>
  
  <script>
import img1 from '/src/assets/rose.webp'
import img2 from '/src/assets/seen.jpg'

  export default {
    data() {
      return {
        name: 'Anushka',
        address: 'Ballabhgarh, sec-1, Haryana',
        dob: '',
        age: null,
        users: [
          { Name: 'Rahul', Age: 24, image: img1 },
          { Name: 'Rohit', Age: 20, image: img2 }
        ]
      };
    },
    methods: {
      calculateAge() {
        if (!this.dob) return;
        const birthYear = new Date(this.dob).getFullYear();
        const currentYear = new Date().getFullYear();
        this.age = currentYear - birthYear;
      },
      validateAge() {
        this.calculateAge();
        this.isUnderAge = this.age < 18;
      }
    }
  };
  </script>
  
<style scoped>

div {
  height: auto;
  width: 500px;
  border: 2px solid violet;
  margin: 0 auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.2);
  background-color: #f9f9ff;
}

h2 {
  color: #5a189a;
  text-align: center;
  font-size: 22px;
  margin-bottom: 15px;
}

input[type="date"] {
  width: 97%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

button {
  display: block;
  width: 100%;
  padding: 10px;
  margin-top: 10px;
  background-color: #5a189a;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: 0.3s;
}

button:hover {
  background-color: #7b2cbf;
}

.error {
  color: red;
  font-weight: bold;
  text-align: center;
  font-size: 18px;
  padding: 10px;
  background-color: #ffd6d6;
  border-radius: 5px;
}

.noerror {
  color: green;
  font-weight: bold;
  text-align: center;
  font-size: 18px;
  padding: 10px;
  background-color: #d6ffd6;
  border-radius: 5px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: white;
  margin: 5px 0;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);
}

img {
  border-radius: 50%;
  border: 2px solid #5a189a;
}

</style>
