<template>
  <div>
    <Header :current_user="current_user" @component="switchComponent" />
    <transition name="fade">
      <component
        v-if="component"
        :is="component"
        :current_user="current_user"
        :selected_email="selected_email"
        @component="switchComponent"
        @reload="getEmails"
      />
    </transition>

    <main>
      <div>
        <article id="filter">
          <form @submit.prevent="getEmails">
            <div>
              <label for="title">{{ $t("title") }}</label>
              <input type="text" name="title" id="title" v-model="title" />
            </div>
            <div>
              <label for="description">{{ $t("description") }}</label>
              <input
                type="text"
                name="description"
                id="description"
                v-model="description"
              />
            </div>
            <button class="svg-button" type="submit">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
                <path
                  d="M500.3 443.7l-119.7-119.7c27.22-40.41 40.65-90.9 33.46-144.7C401.8 87.79 326.8 13.32 235.2 1.723C99.01-15.51-15.51 99.01 1.724 235.2c11.6 91.64 86.08 166.7 177.6 178.9c53.8 7.189 104.3-6.236 144.7-33.46l119.7 119.7c15.62 15.62 40.95 15.62 56.57 0C515.9 484.7 515.9 459.3 500.3 443.7zM79.1 208c0-70.58 57.42-128 128-128s128 57.42 128 128c0 70.58-57.42 128-128 128S79.1 278.6 79.1 208z"
                />
              </svg>
            </button>
            <button class="svg-button" type="reset" @click="resetForm">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
                <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
                <path
                  d="M135.2 17.69C140.6 6.848 151.7 0 163.8 0H284.2C296.3 0 307.4 6.848 312.8 17.69L320 32H416C433.7 32 448 46.33 448 64C448 81.67 433.7 96 416 96H32C14.33 96 0 81.67 0 64C0 46.33 14.33 32 32 32H128L135.2 17.69zM394.8 466.1C393.2 492.3 372.3 512 346.9 512H101.1C75.75 512 54.77 492.3 53.19 466.1L31.1 128H416L394.8 466.1z"
                />
              </svg>
            </button>
          </form>
        </article>

        <article id="main">
          <header>
            <h2>{{ $t("email-templates") }}</h2>
            <div>
              <a href="" @click.prevent="getEmails" class="svg-button">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                  <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
                  <path
                    d="M496 48V192c0 17.69-14.31 32-32 32H320c-17.69 0-32-14.31-32-32s14.31-32 32-32h63.39c-29.97-39.7-77.25-63.78-127.6-63.78C167.7 96.22 96 167.9 96 256s71.69 159.8 159.8 159.8c34.88 0 68.03-11.03 95.88-31.94c14.22-10.53 34.22-7.75 44.81 6.375c10.59 14.16 7.75 34.22-6.375 44.81c-39.03 29.28-85.36 44.86-134.2 44.86C132.5 479.9 32 379.4 32 256s100.5-223.9 223.9-223.9c69.15 0 134 32.47 176.1 86.12V48c0-17.69 14.31-32 32-32S496 30.31 496 48z"
                  />
                </svg>
              </a>
              <a
                href=""
                @click.prevent="component = 'AddEmail'"
                class="svg-button"
              >
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
                  <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
                  <path
                    d="M432 256c0 17.69-14.33 32.01-32 32.01H256v144c0 17.69-14.33 31.99-32 31.99s-32-14.3-32-31.99v-144H48c-17.67 0-32-14.32-32-32.01s14.33-31.99 32-31.99H192v-144c0-17.69 14.33-32.01 32-32.01s32 14.32 32 32.01v144h144C417.7 224 432 238.3 432 256z"
                  />
                </svg>
              </a>
            </div>
          </header>
          <section>
            <table>
              <!-- Head -->
              <thead>
                <tr>
                  <th>{{ $t("title") }}</th>
                  <th>{{ $t("description") }}</th>
                  <th>{{ $t("added") }}</th>
                  <th>{{ $t("actions") }}</th>
                </tr>
              </thead>
              <!-- Content -->
              <tbody v-if="emails">
                <tr v-for="email in emails" :key="email['email_id']">
                  <td>{{ email["title"] }}</td>
                  <td>{{ email["description"] }}</td>
                  <td>{{ email["created_at"].split(".")[0] }}</td>
                  <td>
                    <a
                      href=""
                      @click.prevent="
                        selected_email = email;
                        component = 'CloneEmail';
                      "
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 512 512"
                      >
                        <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
                        <path
                          d="M384 96L384 0h-112c-26.51 0-48 21.49-48 48v288c0 26.51 21.49 48 48 48H464c26.51 0 48-21.49 48-48V128h-95.1C398.4 128 384 113.6 384 96zM416 0v96h96L416 0zM192 352V128h-144c-26.51 0-48 21.49-48 48v288c0 26.51 21.49 48 48 48h192c26.51 0 48-21.49 48-48L288 416h-32C220.7 416 192 387.3 192 352z"
                        />
                      </svg>
                    </a>
                    <a
                      href=""
                      @click.prevent="
                        selected_email = email;
                        component = 'EditEmail';
                      "
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 512 512"
                      >
                        <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
                        <path
                          d="M490.3 40.4C512.2 62.27 512.2 97.73 490.3 119.6L460.3 149.7L362.3 51.72L392.4 21.66C414.3-.2135 449.7-.2135 471.6 21.66L490.3 40.4zM172.4 241.7L339.7 74.34L437.7 172.3L270.3 339.6C264.2 345.8 256.7 350.4 248.4 353.2L159.6 382.8C150.1 385.6 141.5 383.4 135 376.1C128.6 370.5 126.4 361 129.2 352.4L158.8 263.6C161.6 255.3 166.2 247.8 172.4 241.7V241.7zM192 63.1C209.7 63.1 224 78.33 224 95.1C224 113.7 209.7 127.1 192 127.1H96C78.33 127.1 64 142.3 64 159.1V416C64 433.7 78.33 448 96 448H352C369.7 448 384 433.7 384 416V319.1C384 302.3 398.3 287.1 416 287.1C433.7 287.1 448 302.3 448 319.1V416C448 469 405 512 352 512H96C42.98 512 0 469 0 416V159.1C0 106.1 42.98 63.1 96 63.1H192z"
                        />
                      </svg>
                    </a>
                    <a
                      href=""
                      @click.prevent="
                        selected_email = email;
                        component = 'DeleteEmail';
                      "
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 448 512"
                      >
                        <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
                        <path
                          d="M135.2 17.69C140.6 6.848 151.7 0 163.8 0H284.2C296.3 0 307.4 6.848 312.8 17.69L320 32H416C433.7 32 448 46.33 448 64C448 81.67 433.7 96 416 96H32C14.33 96 0 81.67 0 64C0 46.33 14.33 32 32 32H128L135.2 17.69zM394.8 466.1C393.2 492.3 372.3 512 346.9 512H101.1C75.75 512 54.77 492.3 53.19 466.1L31.1 128H416L394.8 466.1z"
                        />
                      </svg>
                    </a>
                  </td>
                </tr>
              </tbody>
              <tbody v-else>
                <tr>
                  <td colspan="4" class="empty-table">
                    Aucune donnée n'a été trouvée.
                  </td>
                </tr>
              </tbody>
            </table>
          </section>
        </article>
      </div>
    </main>
  </div>
</template>

<script>
import Header from "../components/global/Header";
import UserInfo from "../components/session/UserInfo";
import EditPassword from "../components/session/EditPassword";
import AddEmail from "../components/emails/AddEmail";
import CloneEmail from "../components/emails/CloneEmail";
import EditEmail from "../components/emails/EditEmail";
import DeleteEmail from "../components/emails/DeleteEmail";
import { API } from "../assets/js/api";

export default {
  name: "Emails",
  components: {
    Header,
    UserInfo,
    EditPassword,
    AddEmail,
    CloneEmail,
    EditEmail,
    DeleteEmail,
  },
  data() {
    return {
      current_user: JSON.parse(localStorage.getItem("user")),
      component: "",
      emails: [],
      selected_email: [],
      title: "",
      description: "",
    };
  },
  mounted() {
    this.getEmails();
  },
  methods: {
    getEmails() {
      API.getEmails(this.title, this.description)
        .then((response) => {
          this.emails = response;
        })
        .catch(() => {
          this.emails = null;
        });
    },
    resetForm() {
      this.title = this.description = "";
      this.getEmails();
    },
  },
};
</script>
