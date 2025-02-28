<template>
  <form
    class="form-container"
    @submit.prevent="submitForm"
    @keydown.enter.prevent
  >
    <label for="mail">EMAIL:</label>
    <input
      type="text"
      v-model="email"
      name="mail"
      id="mail"
      class="input-field"
      placeholder="Enter email"
    />
    <p v-if="emailError" class="error-text">{{ emailError }}</p>

    <label for="pass">PASSWORD:</label>
    <input
      type="password"
      v-model="password"
      name="pass"
      id="pass"
      class="input-field"
      minlength="8"
      placeholder="Enter password"
    />
    <p v-if="passwordError" class="error-text">{{ passwordError }}</p>

    <label for="role">ROLE:</label>
    <select v-model="role" name="role" id="role" class="input-field">
      <option value="">Select a role</option>
      <option value="Web Developer">Web Developer</option>
      <option value="Web Designer">Web Designer</option>
    </select>
    <p v-if="roleError" class="error-text">{{ roleError }}</p>

    <label for="skill">SKILLS:</label>
    <input
      type="text"
      v-model="newSkill"
      @keyup="handleKeyUp"
      @blur="validateSkillOnBlur"
      placeholder="Enter or edit skills"
      class="input-field"
    />
    <p v-if="skillsError" class="error-text">{{ skillsError }}</p>
    <div class="skills-container">
      <span v-for="(skill, index) in skills" :key="index" class="skill-box">
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
      return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/.test(
        this.password
      );
    }
  },
  watch: {
    email(value) {
      const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
      this.emailError = !value.trim()
        ? 'Email is required.'
        : !emailPattern.test(value)
        ? 'Please enter a valid email.'
        : '';
    },
    password(value) {
      this.passwordError = !value.trim()
        ? 'Password is required.'
        : !this.isPasswordValid
        ? 'Password must be 8 chars long, include 1 uppercase, 1 lowercase, 1 number, and 1 special character.'
        : '';
    },
    role(value) {
      this.roleError = value ? '' : 'Please select a role.';
    },
    terms(value) {
      this.termsError = value ? '' : 'You must accept the terms and conditions.';
    }
  },
  methods: {
    handleKeyUp(event) {
      if (event.key === ',' || event.key === 'Enter') {
        this.newSkill = this.newSkill.replace(/,$/, '').trim();
        if (this.newSkill !== '') {
          if (this.editingIndex !== null) {
            this.skills.splice(this.editingIndex, 1, this.newSkill);
            this.editingIndex = null;
          } else {
            this.skills.push(this.newSkill);
          }
          this.newSkill = '';
          this.skillsError = '';
        } else if (this.editingIndex !== null) {
          this.skillsError = 'Skill cannot be empty.';
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
      this.skillsError = this.skills.length > 0 ? '' : 'Please enter at least one skill.';
    },
    validateSkillOnBlur() {
      if (this.editingIndex !== null && this.newSkill.trim() === '') {
        this.skillsError = 'Skill cannot be empty.';
      }
    },
    validateForm() {
      const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
      this.emailError = !this.email.trim()
        ? 'Email is required.'
        : !emailPattern.test(this.email)
        ? 'Please enter a valid email.'
        : '';

      this.passwordError = !this.password.trim()
        ? 'Password is required.'
        : !this.isPasswordValid
        ? 'Password must be 8 chars long, include 1 uppercase, 1 lowercase, 1 number, and 1 special character.'
        : '';
      
      this.roleError = this.role ? '' : 'Please select a role.';

      this.skillsError = this.skills.length > 0 ? '' : 'Please enter at least one skill.';
      this.termsError = this.terms ? '' : 'You must accept the terms and conditions.';

      return !(
        this.emailError ||
        this.passwordError ||
        this.roleError ||
        this.skillsError ||
        this.termsError
      );
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
