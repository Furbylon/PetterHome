<template>
  <nb-container>
    <AppHeader screen="Recover Password" />
    <nb-content>
      <nb-form>
        <InputWithError
          class="textInput"
          :error="
            $v.form.usernameConfirmation.$dirty &&
            !$v.form.usernameConfirmation.sameAsUsername
          "
          msg="Username doesn't match any in the register!"
        >
          <nb-input
            v-model="form.usernameConfirmation"
            placeholder="Username"
            auto-captialize="none"
            :on-blur="() => $v.form.usernameConfirmation.$touch()"
          />
        </InputWithError>
        <InputWithError
          class="textInput"
          :error="
            $v.form.oldPasswordConfirmation.$dirty &&
            !$v.form.oldPasswordConfirmation.sameAsOldPassword
          "
          msg="Must match old password!"
        >
          <nb-input
            secure-text-entry
            v-model="form.passwordConfirmation"
            placeholder="Old Password Confirmation"
            :on-blur="() => $v.form.oldPasswordConfirmation.$touch()"
          />
        </InputWithError>
        <InputWithError
          class="textInput"
          :error="$v.form.newPassword.$dirty && !$v.form.newPassword.required"
          msg="Password is required!"
        >
          <nb-input
            secure-text-entry
            v-model="form.password"
            placeholder="New Password"
            :on-blur="() => $v.form.newPassword.$touch()"
          />
        </InputWithError>
        <InputWithError
          class="textInput"
          :error="
            $v.form.newPasswordConfirmation.$dirty &&
            !$v.form.newPasswordConfirmation.sameAsNewPassword
          "
          msg="Password Confirmation must match new password!"
        >
          <nb-input
            secure-text-entry
            v-model="form.password"
            placeholder="New Password Confirmation"
            :on-blur="() => $v.form.newPasswordConfirmation.$touch()"
          />
        </InputWithError>
        <nb-button class="button" block :on-press="updatePassword"
          ><nb-text>Update</nb-text>
        </nb-button>
        <nb-button class="subButton" transparent :on-press="goToLogin"
          ><nb-text class="text">Return to login</nb-text></nb-button
        >
      </nb-form>
    </nb-content>
  </nb-container>
</template>

<script>
import { required, sameAs } from "vuelidate/lib/validators";
import { Toast } from "native-base";
import user from "../data/userMock.json";
export default {
  props: {
    navigation: {
      type: Object,
    },
  },
  data() {
    return {
      user: user,
      form: {
        username: user.username,
        usernameConfirmation: "",
        oldPassword: user.password,
        oldPasswordConfirmation: "",
        newPassword: "",
        newPasswordConfirmation: "",
      },
    };
  },

  validations: {
    form: {
      usernameConfirmation: {
        required,
        sameAsUsername: sameAs("username"),
      },
      oldPasswordConfirmation: {
        required,
        sameAsOldPassword: sameAs("oldPassword"),
      },
      newPassword: { required },
      newPasswordConfirmation: {
        sameAsNewPassword: sameAs("newPassword"),
      },
    },
  },
  methods: {
    updatePassword() {
      this.$v.form.$touch();
      if (this.$v.form.$invalid) {
        this.$store
          .dispatch("auth/updatePassword")
          .then(() => this.navigateToLogin())
          .catch(() => {
            Toast.show({
              text: "something went wrong",
              buttonText: "okay",
              type: "danger",
              duration: 3000,
            });
          });
      }
      alert(JSON.stringify(this.form));
    },
    goToLogin() {
      this.navigation.navigate("Login");
    },
    navigateToLogin() {
      this.navigation.navigate("Login", {
        message: "Succesfull password update",
      });
    },
  },
};
</script>
<style scoped>
.button {
  border-radius: 20;
  margin-top: 10;
  margin-right: 40;
  margin-left: 40;
  background-color: #edccaf;
}
.subButton {
  border-radius: 20;
  margin-top: 10;
  margin-right: 50;
  margin-left: 130;
  background-color: #edccaf;
}
.textInput {
  margin-right: 40;
  margin-left: 40;
  font-family: ArchitectsDaughter;
}
.text {
  font-family: ArchitectsDaughter;
  color: white;
}
</style>