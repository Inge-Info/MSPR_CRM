<template>
  <div class="popup">
    <article>
      <header>
        <h2>{{ $t("send-email-user") }} {{ full_name }}</h2>
      </header>

      <section>
        <form @submit.prevent="sendEmail">
          <label for="email">{{ $t("email") }}</label>
          <select name="email" id="email" v-model="email_id" required>
            <option value="" selected>{{ $t("select-email") }}</option>
            <option
              v-for="email in emails"
              :key="email['email_id']"
              :value="email['email_id']"
            >
              {{ email["title"] }}
            </option>
          </select>
          <div>
            <button type="submit" class="button">
              {{ $t("send") }}
            </button>
            <button
              type="button"
              class="button btn-back"
              @click="$emit('component', { name: '' })"
            >
              {{ $t("close") }}
            </button>
          </div>
        </form>
      </section>
    </article>
  </div>
</template>

<script>
import { API } from "../../assets/js/api";

export default {
  name: "SendEmailUser",
  props: ["selected_user"],
  data() {
    return {
      emails: {},
      email_id: "",
    };
  },
  mounted() {
    this.addEvents("", document.getElementsByClassName("popup")[0]);

    API.getEmails("", "").then((response) => {
      this.emails = response;
    });
  },
  beforeUnmount() {
    this.removeEvents("", document.getElementsByClassName("popup")[0]);
  },
  methods: {
    sendEmail: function () {
      API.sendEmails(this.email_id, [this.selected_user["user_id"]])
        .then(() => {
          this.$notify(
            {
              group: "success",
              title: this.$t("email-send-user"),
              text: this.$t("email-send-user-msg"),
            },
            3500
          );
          this.$emit("component", { name: "" });
        })
        .catch(() => {
          this.$notify(
            {
              group: "error",
              title: this.$t("error"),
              text: this.$t("email-send-user-error"),
            },
            3500
          );
          this.$emit("component", { name: "" });
        });
    },
  },
  computed: {
    full_name: function () {
      return (
        this.selected_user["first_name"] + " " + this.selected_user["last_name"]
      );
    },
  },
};
</script>
