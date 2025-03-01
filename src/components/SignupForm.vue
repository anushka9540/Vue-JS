<template>
  <form class="form-container" @submit.prevent="submitForm">
    <label for="mail">EMAIL:</label>
    <input type="text" v-model="formData.email" id="mail" class="input-field" placeholder="Enter email"
      @input="validateField('email')" />
    <p v-if="formErrors.email" class="error-text">{{ formErrors.email }}</p>
  
    <label for="pass">PASSWORD:</label>
    <input type="password" v-model="formData.password" id="pass" class="input-field" placeholder="Enter password"
      @input="validateField('password')" />
    <p v-if="formErrors.password" class="error-text">
      {{ formErrors.password }}
    </p>
  
    <label for="role">ROLE:</label>
    <select v-model="formData.role" id="role" class="input-field" @change="validateField('role')">
      <option value="">Select a role</option>
      <option value="Web Developer">Web Developer</option>
      <option value="Web Designer">Web Designer</option>
    </select>
    <p v-if="formErrors.role" class="error-text">{{ formErrors.role }}</p>
  
    <label for="skill">SKILLS:</label>
    <input type="text" v-model="formData.newSkill" @keyup.enter="handleSkill" @keyup="addSkillsOnComma"
      @input="validateSkill" @blur="validateSkill" placeholder="Enter or edit skills" class="input-field" />
    <p v-if="formErrors.skills" class="error-text">{{ formErrors.skills }}</p>
    <div class="skills-container">
      <span v-for="(skill, index) in formData.skills" :key="index" class="skill-box">
        {{ skill }}
        <button type="button" class="edit-btn" @click="editSkill(index)">
          ✏️
        </button>
        <button type="button" class="remove-btn" @click="removeSkill(index)">
          ✖
        </button>
      </span>
    </div>
  
    <div class="checkbox-container">
      <input type="checkbox" v-model="formData.terms" id="term" @change="validateField('terms')" />
      <label for="term">ACCEPT TERMS AND CONDITIONS</label>
    </div>
    <p v-if="formErrors.terms" class="error-text check-box-error">
      {{ formErrors.terms }}
    </p>
  
    <button type="submit" class="submit-button">Create an Account</button>
  </form>
</template>

<script>
export default {
  name: 'SignupForm',
  data() {
    return {
      formData: {
        email: '',
        password: '',
        role: '',
        newSkill: '',
        skills: [],
        terms: false
      },
      formErrors: {
        email: '',
        password: '',
        role: '',
        skills: '',
        terms: ''
      },
      editingIndex: null
    };
  },
  methods: {
    checkPasswordValid() {
      return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/.test(
        this.formData.password
      );
    },

    validateField(field) {
      switch (field) {
        case 'email':
          this.formErrors.email = !this.formData.email.trim()
            ? 'Email is required.'
            : /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/.test(
              this.formData.email
            )
              ? ''
              : 'Please enter a valid email.';
          break;
        case 'password':
          this.formErrors.password = !this.formData.password.trim()
            ? 'Password is required.'
            : this.checkPasswordValid()
              ? ''
              : 'Password must be 8 chars long, include 1 uppercase, 1 lowercase, 1 number, and 1 special character.';
          break;
        case 'role':
          this.formErrors.role = this.formData.role
            ? ''
            : 'Please select a role.';
          break;
        case 'terms':
          this.formErrors.terms = this.formData.terms
            ? ''
            : 'You must accept the terms and conditions.';
          break;
      }
    },
    validateSkill() {
      if (this.formData.newSkill.trim()) {
        this.formErrors.skills = '';
      } else if (this.editingIndex !== null || !this.formData.skills.length) {
        this.formErrors.skills = 'Skill cannot be empty.';
      }
    },
    handleSkill() {
      this.formData.newSkill = this.formData.newSkill.replace(/,$/, '').trim();
      if (this.formData.newSkill) {
        this.editingIndex !== null
          ? this.formData.skills.splice(
            this.editingIndex,
            1,
            this.formData.newSkill
          )
          : this.formData.skills.push(this.formData.newSkill);
        this.formData.newSkill = '';
        this.editingIndex = null;
        this.formErrors.skills = '';
      } else if (this.editingIndex !== null) {
        this.formErrors.skills = 'Skill cannot be empty.';
      }
    },
    addSkillsOnComma(event) {
      if (event.key === ',') {
        this.handleSkill();
        event.preventDefault();
      }
    },
    editSkill(index) {
      this.formData.newSkill = this.formData.skills[index];
      this.formData.skills.splice(index, 1);
      this.editingIndex = index;
    },
    removeSkill(index) {
      this.formData.skills.splice(index, 1);
      this.formErrors.skills = this.formData.skills.length
        ? ''
        : 'Please enter at least one skill.';
    },
    validateForm() {
      Object.keys(this.formData).forEach((field) => {
        if (field in this.formErrors) {
          this.validateField(field);
        }
      });
      this.validateSkill();
      return !Object.values(this.formErrors).some((error) => error);
    },
    submitForm() {
      if (this.validateForm()) {
        console.log('Form submitted:', this.formData);
        alert('Form submitted successfully!');
        this.formData = {
          email: '',
          password: '',
          role: '',
          newSkill: '',
          skills: [],
          terms: false
        };
        Object.keys(this.formErrors).forEach(
          (key) => (this.formErrors[key] = '')
        );
      }
    }
  }
};
</script>

<style scoped>
.form-container {
  width: 400px;
  padding: 40px;
  background: linear-gradient(135deg, #f8fafc, #e2e8f0);
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
  color: black;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 8px;
}

label {
  font-size: 12px;
  font-weight: 700;
  color: #374151;
}

.input-field {
  width: 94%;
  padding: 12px;
  border-radius: 15px;
  background: #ffffff;
  border: 1px solid #cbd5e1;
  color: #374151;
  font-size: 14px;
  transition: 0.3s;
}

#role {
  width: 100%;
}

.input-field::placeholder {
  color: #94a3b8;
}

.input-field:focus {
  border-color: #007bff;
  background: #f8fafc;
  outline: none;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.4);
}

.error-text {
  color: red;
  font-size: 11px;
  margin-top: -3px;
  width: 100%;
  text-align: left;
}

.skills-container {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  padding: 5px;
}

.skill-box {
  background: #dbeafe;
  padding: 6px 14px;
  border-radius: 18px;
  font-size: 13px;
  font-weight: 600;
  display: flex;
  align-items: center;
  color: #1e3a8a;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: 0.3s;
}

.skill-box:hover {
  background: #bfdbfe;
}

.edit-btn,
.remove-btn {
  background: none;
  border: none;
  font-size: 14px;
  cursor: pointer;
  transition: 0.3s;
  display: flex;
  align-items: center;
  margin-left: 7px;
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
  padding: 12px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  transition: 0.3s;
}

.submit-button:hover {
  background: #0056b3;
  transform: scale(1.03);
}

@media (max-width: 768px) {
  .form-container {
    width: 80%;
    padding: 25px;
  }

  .input-field {
    padding: 10px;
  }

  .submit-button {
    font-size: 14px;
  }
}
</style>
