<template>
  <form class="form-container" @submit.prevent="submitForm">
    <label for="mail">EMAIL:</label>
    <input type="email" v-model="email" name="mail" id="mail" required class="input-field" />
  
    <label for="pass">PASSWORD:</label>
    <input type="password" v-model="password" name="pass" id="pass" required class="input-field" maxlength="8" />
  
    <label for="role">ROLE:</label>
    <select v-model="role" name="role" id="role" class="input-field" required>
      <option value="Web Developer">Web Developer</option>
      <option value="Web Designer">Web Designer</option>
    </select>
  
    <label for="skill">SKILLS:</label>
    <input 
        type="text" 
        v-model="newSkill" 
        @keyup="handleKeyUp" 
        placeholder="Enter skills" 
        class="input-field"
      />
    
    <div class="skills-container">
      <span v-for="(skill, index) in skills" :key="index" class="skill-box">
        {{ skill }}
        <button type="button" class="remove-btn" @click="removeSkill(index)">✖</button>
      </span>
    </div>

    <div class="checkbox-container">
      <input type="checkbox" v-model="terms" id="term" />
      <label for="term">ACCEPT TERMS AND CONDITIONS</label>
    </div>
  
    <button type="submit" class="submit-button" @keydown.enter.prevent>
      Create an Account
    </button>
  </form>
</template>

<script>
export default {
  name: 'SignupForm',
  data() {
    return {
      email: '',
      password: '',
      role: 'Web Developer',
      newSkill: '',
      skills: [],
      terms: false
    };
  },
  methods: {
    handleKeyUp(event) {
      if (event.key === ',' || event.key === 'Enter') {
        this.newSkill = this.newSkill.replace(/,$/, '');
        if (this.newSkill.trim() !== '') {
          this.skills.push(this.newSkill.trim());
          this.newSkill = '';
        }
        event.preventDefault();
      }
    },
    removeSkill(index) {
      this.skills.splice(index, 1);
    },
    submitForm() {
      if (!this.terms) {
        alert('You must accept the terms and conditions.');
        return;
      }
      console.log('Form submitted:', {
        email: this.email,
        password: this.password,
        role: this.role,
        skills: this.skills,
        terms: this.terms
      });
    }
  }
};
</script>

<style scoped>
.form-container {
  width: 360px;
  padding: 50px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  color: black;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 6px;
  margin: auto;
}

label {
  font-size: 12px;
  font-weight: 700;
}

.input-field {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: none;
  border-radius: 5px;
  background: transparent;
  color: black;
  border-bottom: 1px solid #ccc;
}

.skills-container {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  padding: 5px;
  border-radius: 5px;
  min-height: 40px;
  margin-top: -6px;
  gap: 5px;
}

.skill-box {
  background: #eee;
  padding: 5px 8px;
  border-radius: 20px;
  font-size: 12px;
  display: flex;
  align-items: center;
  cursor: pointer;
}

.remove-btn {
  background: transparent;
  border: none;
  color: red;
  font-size: 14px;
  cursor: pointer;
}

.remove-btn:hover {
  color: darkred;
}

.checkbox-container {
  display: flex;
  align-items: center;
  gap: 5px;
  margin-top: -3px;
}

.submit-button {
  width: 100%;
  padding: 10px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  font-size: 16px;
}

.submit-button:hover {
  background: #0056b3;
}

@media (max-width: 768px) {
  .form-container {
    width: 80%;
    padding: 30px;
  }

  .input-field {
    padding: 8px;
  }

  .submit-button {
    font-size: 14px;
  }
}
</style>

