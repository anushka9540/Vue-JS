<template>
  <form class="form-container" @submit.prevent="submitForm" @keydown.enter.prevent>
    <label for="mail">EMAIL:</label>
    <input type="text" v-model="email" name="mail" id="mail" class="input-field" />
    <p v-if="emailError" class="error-text">{{ emailError }}</p>
  
    <label for="pass">PASSWORD:</label>
    <input type="password" v-model="password" name="pass" id="pass" class="input-field" maxlength="8" />
    <p v-if="passwordError" class="error-text">{{ passwordError }}</p>
  
    <label for="role">ROLE:</label>
    <select v-model="role" name="role" id="role" class="input-field">
      <option value="">Select a role</option>
      <option value="Web Developer">Web Developer</option>
      <option value="Web Designer">Web Designer</option>
    </select>
    <p v-if="roleError" class="error-text">{{ roleError }}</p>
  
    <label for="skill">SKILLS:</label>
    <input type="text" v-model="newSkill" @keyup="handleKeyUp" placeholder="Enter or edit skills" class="input-field" />
  
    <div class="skills-container">
      <span v-for="(skill, index) in skills" :key="index" class="skill-box">
        {{ skill }}
        <button type="button" class="edit-btn" @click="editSkill(index)">✏️</button>
        <button type="button" class="remove-btn" @click="removeSkill(index)">✖</button>
      </span>
    </div>
    <p v-if="skillsError" class="error-text">{{ skillsError }}</p>
  
    <div class="checkbox-container">
      <input type="checkbox" v-model="terms" id="term" />
      <label for="term">ACCEPT TERMS AND CONDITIONS</label>
    </div>
    <p v-if="termsError" class="error-text check-box-error">{{ termsError }}</p>
  
    <button type="submit" class="submit-button">Create an Account</button>
  </form>
</template>

<script>
export default {
  name: 'SignupForm',
  data() {
    return {
      email: '',
      password: '',
      role: '',
      newSkill: '',
      skills: [],
      editingIndex: null,
      terms: false,
      emailError: '',
      passwordError: '',
      roleError: '',
      skillsError: '',
      termsError: ''
    };
  },
  computed: {
    isPasswordValid() {
      return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8}$/.test(this.password);
    }
  },
  methods: {
    handleKeyUp(event) {
      if (event.key === ',' || event.key === 'Enter') {
        this.newSkill = this.newSkill.replace(/,$/, '').trim();
        if (this.newSkill !== '') {
          if (this.editingIndex !== null) {
            this.skills[this.editingIndex] = this.newSkill;
            this.editingIndex = null;
          } else {

            this.skills.push(this.newSkill);
          }
          this.newSkill = '';
        }
        event.preventDefault();
      }
    },
    editSkill(index) {
      this.newSkill = this.skills[index];
      this.skills.splice(index, 1);
      this.editingIndex = index;
    },
    removeSkill(index) {
      this.skills.splice(index, 1);
    },
    validateForm() {
      this.emailError = this.email.includes('@') ? '' : 'Invalid email format include @.';
      this.passwordError = this.isPasswordValid ? '' : 'Password must be 8 chars long, include 1 uppercase, 1 lowercase, 1 number, and 1 special character.';
      this.roleError = this.role ? '' : 'Please select a role.';
      this.skillsError = this.skills.length > 0 ? '' : 'Please enter at least one skill.';
      this.termsError = this.terms ? '' : 'You must accept the terms and conditions.';

      return !(this.emailError || this.passwordError || this.roleError || this.skillsError || this.termsError);
    },
    submitForm() {
      if (this.validateForm()) {
        console.log('Form submitted:', {
          email: this.email,
          password: this.password,
          role: this.role,
          skills: this.skills,
          terms: this.terms
        });
        alert('Form submitted successfully!');
      }
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
  font-size: 11px;
  font-weight: 800;
}

.input-field {
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 5px;
  background: #fff;
  color: black;
  border-bottom: 1px solid #ccc;
  margin-top: 2px;
  margin-bottom: 7px;
}

.error-text {
  color: red;
  font-size: 10px;
  margin-top: -3px;
}

.skills-container {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  padding: 5px;
}

.skill-box {
  background: #f3f4f6;
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 500;
  display: flex;
  align-items: center;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  transition: 0.2s;
}

.skill-box:hover {
  background: #e5e7eb;
}

.edit-btn,
.remove-btn {
  background: none;
  border: none;
  font-size: 12px;
  cursor: pointer;
  transition: 0.2s;
  display: flex;
  align-items: center;
  margin-left: 5px;
}

.edit-btn {
  color: #007bff;
  padding: 0;
}

.edit-btn:hover {
  color: #0056b3;
}

.remove-btn {
  color: #e63946;
  padding: 0;
}

.remove-btn:hover {
  color: #b71c1c;
}

.checkbox-container {
  display: flex;
  align-items: center;
  gap: 5px;
  margin-top: 8px;
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
