<template>
  <h1 v-if="activePhase === 1">
    Ahoy you!<br />
    <span>Care to register?</span>
  </h1>
  <h1 v-if="activePhase === 2">
    Great!<br />
    <span>Now your name</span>
  </h1>

  <div class="form-container" v-if="activePhase === 1 || activePhase === 2">
    <form @submit.prevent="submitForm">
      <div class="tab" id="form-step1" v-if="activePhase === 1">
        <div class="form-group">
          <label for="user-email">EMAIL</label>
          <input
            type="email"
            name="user-email"
            id="user-email"
            placeholder="Enter your email adress"
            v-model.trim="user.email"
            required
          />
        </div>

        <div class="form-group">
          <label for="user-password">PASSWORD</label>
          <div class="input-container">
            <input
              :type="inputType"
              name="user-password"
              id="user-password"
              placeholder="Enter your password"
              autocomplete="off"
              v-model="user.password"
              required
            />
            <button @click="togglePasswordInput">
              <img src="/img/eye.svg" alt="" />
            </button>
          </div>
        </div>
        <div class="form-validation">
          <p :class="passwordLengthValidation">At least 8 characters</p>
          <p :class="passwordLetterValidation">At least one letter</p>
          <p :class="passwordDigitValidation">At least one digit</p>
        </div>
        <div class="form-row">
          <a href="#" class="btn btn-secondary">Log in instead</a>
          <button @click.self="nextStep" class="btn btn-primary">
            Next Step
          </button>
        </div>
      </div>
      <!-- step 2 -->
      <div class="tab" id="form-step2" v-if="activePhase === 2">
        <div class="form-group">
          <label for="user-name">FIRST NAME</label>
          <input
            type="text"
            name="user-name"
            id="user-name"
            placeholder="e.g. Jessica"
            v-model.trim="user.name"
            required
          />
        </div>

        <div class="form-group">
          <label for="user-lastname">LAST NAME</label>
          <input
            type="text"
            name="user-lastname"
            id="user-lastname"
            placeholder="e.g. Walton"
            v-model.trim="user.lastName"
            required
          />
        </div>

        <div class="form-group">
          <label for="birth-date">DATE OF BIRTH</label>
          <input
            type="date"
            name="birth-date"
            id="birth-date"
            placeholder="DD / MM / YYYY"
            min="1900-01-01"
            max="2050-12-31"
            v-model="user.birthDate"
            required
          />
          <p :class="userAge">You should be minimum 18 years old</p>
        </div>

        <div class="checkbox">
          <input
            class="custom-checkbox"
            id="confirm-terms"
            name="confirm-terms"
            type="checkbox"
            value="confirmed"
            required
            v-model="user.confirmTerms"
          />
          <span class="checkmark"></span>
          <label for="confirms-terms">
            I accept <a href="#">Privacy Policy</a></label
          >
        </div>

        <div class="form-row">
          <a href="#" class="btn btn-secondary">Log in instead</a>
          <button
            type="submit"
            @click.self="submitForm"
            class="btn btn-primary"
          >
            Register
          </button>
        </div>
      </div>
    </form>
  </div>
  <form-success v-if="activePhase === 3" :data="user"></form-success>
</template>

<script>
import FormSuccess from "./FormSuccess.vue";
export default {
  components: {
    FormSuccess,
  },

  data() {
    return {
      activePhase: 1,
      user: {
        email: "",
        password: "",
        name: "",
        lastName: "",
        birthDate: "",
        confirmTerms: false,
      },
      validation: {
        email: false,
        password: false,
        name: false,
        lastName: false,
        birthDate: false,
        confirm: false,
      },
        inputType: "password",
    };
  },

  computed: {
    passwordLengthValidation() {
      if (this.user.password != "" && this.user.password.length < 8) {
        return "invalid";
      } else if (this.user.password != "" && this.user.password.length >= 8)
        return "valid";
    },
    passwordLetterValidation() {
      let re = /[a-zA-Z]/;
      const result = re.test(this.user.password);
      if (this.user.password != "" && result === true) {
        return "valid";
      } else if (this.user.password != "" && result === false) return "invalid";
    },
    passwordDigitValidation() {
      let re = /[0-9]/;
      const result = re.test(this.user.password);
      if (this.user.password != "" && result === true) {
        return "valid";
      } else if (this.user.password != "" && result === false) return "invalid";
    },
    
    userAge() {
      if (this.user.birthDate != "" && this.validation.birthDate === false) {
        return "invalid";
      }
    },
  },

  methods: {
    togglePasswordInput() {
      if (this.user.password != "" && this.inputType === "password") {
        this.inputType = "text";
      } else if (this.user.password != "" && this.inputType === "text") {
        this.inputType = "password";
      } else this.inputType = "password";
    },

    nextStep() {
      if (
        this.emailValidation() &&
        this.passwordValidation() &&
        this.activePhase === 1
      ) {
        this.activePhase = 2;
      }
    },

    formValidation() {
      this.emailValidation();
      this.passwordValidation();
      this.nameValidation();
      this.lastNameValidation();
      this.ageValidation();
      this.termsValidation();
    },

    emailValidation() {
      const re = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/;
      const result = re.test(this.user.email);
      if (result === true) {
        this.validation.email = true;
        return true;
      } else this.validation.email = false;
      return false;
    },

    passwordValidation() {
      let re = /[0-9a-zA-Z]{8,18}/;
      const result = re.test(this.user.password);
      if (result === true) {
        this.validation.password = true;
        return true;
      } else this.validation.password = false;
      return false;
    },

    nameValidation() {
      let re = /([A-Za-zżźćńółęąśŻŹĆĄŚĘŁÓŃ]+[ ]*)+/;
      const result = re.test(this.user.name);
      if (result === true) {
        this.validation.name = true;
        return true;
      } else this.validation.name = false;
      return false;
    },

    lastNameValidation() {
      let re = /([A-Za-zżźćńółęąśŻŹĆĄŚĘŁÓŃ]+[ ]*)+/;
      const result = re.test(this.user.lastName);
      if (result === true) {
        this.validation.lastName = true;
        return true;
      } else this.validation.lastName = false;
      return false;
    },

    ageValidation() {
      const userBD = this.user.birthDate;
      const re = /^\d{4}[\/\-](0?[1-9]|1[012])[\/\-](0?[1-9]|[12][0-9]|3[01])$/;

      if (re.test(userBD)) {
        const dateParts = userBD.split("-");
        const currentDate = new Date();

        if (currentDate.getFullYear() - dateParts[0] < 18) {
          return false;
        }

        if (currentDate.getFullYear() - dateParts[0] == 18) {
          if (currentDate.getMonth() + 1 < dateParts[1]) {
            return false;
          }
          if (currentDate.getMonth() + 1 == dateParts[1]) {
            if (currentDate.getDate() < dateParts[2]) {
              return false;
            }
          }
        }
        this.validation.birthDate = true;
        return true;
      } else {
        this.validation.birthDate = false;
        return false;
      }
    },

    termsValidation() {
      if (this.user.confirmTerms === true) {
        this.validation.confirm = true;
        return true;
      } else this.validation.confirm = false;
      return false;
    },

    submitForm() {
      this.formValidation();

      for (const key of Object.keys(this.validation)) {
        if (this.validation[key] === false) {
          return false;
        }
      }
      this.activePhase = 3;
    },
  },
};
</script>

<style scoped>
h1 span {
  color: rgb(133, 134, 141);
}

.form-group p {
  font-family: "Roboto";
  margin-top: 8px;
  font-size: 14px;
  line-height: 170%;
}

.checkbox {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 10px;
  margin-bottom: 40px;
}
/* Custom checkbox */
.custom-checkbox {
  -webkit-appearance: none;
  border: 0.75px solid #aeaeb3;
  width: 24px;
  height: 24px;
  border-radius: 8px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05),
    inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05);
  display: inline-block;
  position: relative;
}

.custom-checkbox:active,
.custom-checkbox:checked:active {
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05),
    inset 0px 1px 3px rgba(0, 0, 0, 0.1);
}

.custom-checkbox:checked {
  border: 2px solid #a60c0e;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05),
    inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05),
    inset 15px 10px -12px rgba(255, 255, 255, 0.1);
  color: #a60c0e;
}

.custom-checkbox:checked:after {
  content: "\2714";
  font-size: 15px;
  position: absolute;
  top: 0px;
  left: 4px;
  color: #a60c0e;
}

.checkbox label {
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  line-height: 21px;
  /* margin-left: 35px; */
  font-size: 18px;
  text-transform: none;
  color: #292a33;
}

.checkbox a {
  color: #292a33;
  text-decoration: underline;
}
</style>
