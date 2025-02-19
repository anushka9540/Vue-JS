<template>
    <div>
      <h2>User Information</h2>
      <p>Name: {{ name }}</p>
      <p>Address: {{ address }}</p>
      <label for="dob">Enter Date of Birth:</label>
      <input type="date" v-model="dob" id="dob" />
      <button @click="calculateAge">Calculate Age</button>
      <p v-if="age !== null">Age: {{ age }}</p>

      <p v-if="age !== null" :class="age < 18 ? 'error' : 'success'" class="agetext">
      {{ age < 18 ? 'You are under age' : 'You are OK to use the website' }}
       </p>
      
      <h2>Users List</h2>
      <ul>
        <li v-for="user in users" :key="user.Name">
          <p>UserName: {{ user.name }}, Age: {{ user.age }}</p>
          <img :src="user.image" alt="User Image" width="100" />
        </li>
      </ul>
    </div>
  </template>
  
  <script>
import roseimage from '/src/assets/rose.webp'
import natureimage from '/src/assets/seen.jpg'

  export default {
    data() {
      return {
        name: 'Anushka',
        address: 'Ballabhgarh, sec-1, Haryana',
        dob: '',
        age: null,
        users: [
          { name: 'Rahul', age: 24, image: roseimage },
          { name: 'Rohit', age: 20, image: natureimage }
        ]
      };
    },
    methods: {
        calculateAge() {
            if (!this.dob) return;
            const birthDate = new Date(this.dob);
            const today = new Date();

            let age = today.getFullYear() - birthDate.getFullYear();
            const monthDiff = today.getMonth() - birthDate.getMonth();
            const dayDiff = today.getDate() - birthDate.getDate();

            if (monthDiff < 0 || (monthDiff === 0 && dayDiff < 0)) {
                age--;
            }

            this.age = age;
        }

    }
  };
  </script>
  
<style scoped>

div {
  max-width: 90%;
  width: 500px;
  border: 2px solid violet;
  margin: 20px auto;
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
  width: 95%;
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

.agetext {
  display: block;
  width: 100%;
  text-align: center;
  padding: 10px;
  font-weight: bold;
  border-radius: 5px;
}

.error {
  color: red;
  background-color: #ffd6d6;
}

.success {
  color: green;
  background-color: #d6ffd6;
}

.agetext {
  font-size: 18px;
  padding: 10px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  flex-wrap: wrap;
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
  width: 80px;
  height: 80px;
  object-fit: cover;
}

@media screen and (max-width: 600px) {
  div {
    width: 95%;
    padding: 15px;
  }

  h2 {
    font-size: 20px;
  }

  input[type="date"] {
    font-size: 14px;
  }

  button {
    font-size: 14px;
  }

  .agetext {
    font-size: 16px;
    padding: 8px;
  }

  li {
    flex-direction: column;
    text-align: center;
    padding: 15px;
  }

  img {
    width: 70px;
    height: 70px;
    margin-top: 10px;
  }
}


</style>
