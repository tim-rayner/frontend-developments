<script setup lang="ts">
import { reactive } from "vue";
import ButtonSpinner from "@/components/animation/ButtonSpinner.vue";
import { isValidEmail } from "@/helpers/email-helpers";
const data = reactive({
  form: {
    name: "",
    company: "",
    email: "",
    message: "",
    submitted: false as boolean,
  },
  submission: false,
  loading: false,
});

const validateForm = () => {
  if (
    data.form.name.length &&
    data.form.company.length &&
    //check if email is valid format
    isValidEmail(data.form.email) &&
    data.form.message.length
  ) {
    return true;
  } else {
    if (!isValidEmail(data.form.email)) {
      data.form.email = "";
    }
    return false;
  }
};

const submitForm = (e: event) => {
  e.preventDefault();
  data.submission = true;

  data.loading = true;

  if (validateForm()) {
    setTimeout(() => {
      data.form.submitted = true;
      console.warn(
        "sorry guys, this form doesn't actually submit anywhere. this is just a demo."
      );
      data.loading = false;
    }, 1000);
  } else {
    data.loading = false;
  }
};
</script>

<template>
  <div
    class="hero flex flex-col md:flex-row container mx-auto min-h-[100vh] items-center"
  >
    <div class="content md:w-1/2 text-center md:text-left h-full">
      <h1>Let's Work <strong>Together</strong></h1>
      <p>Start your solar journey today</p>
      <div class="img-container container">
        <!-- <img
          src="https://cdn.discordapp.com/attachments/1119779286174408716/1130059223594709002/rayn3r_a_company_meeting_for_a_solar_panel_company_hosted_at_a_b30e7ba9-90aa-42d7-b45c-891ed5e07849.png"
          alt="Solar Vista company meeting"
          class="rounded-xl h-[423.75px] w-3/4 m-auto md:m-0"
        /> -->
      </div>
    </div>

    <div class="form border-l w-full md:w-1/2 px-5 md:mt-[80px] mx-auto h-full">
      <form id="home-contact-form" class="container w-3/4 lg:w-full m-auto">
        <div class="flex flex-wrap -mx-3 mb-6">
          <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
            <label
              class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
              for="grid-first-name"
            >
              Name<span :class="{ 'text-red-500': !data.form.name.length }">
                &nbsp;*</span
              >
            </label>
            <input
              class="appearance-none block w-full bg-gray-200 text-gray-700 border rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white"
              id="grid-first-name"
              type="text"
              placeholder="Jane"
              v-model="data.form.name"
              :class="{
                'border-red-500': !data.form.name.length && data.submission,
              }"
            />
            <p
              v-if="!data.form.name.length && data.submission"
              class="text-red-500 text-xs italic"
            >
              Please fill out this field.
            </p>
          </div>

          <div class="w-full md:w-1/2 mb-6 px-3">
            <label
              class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
              for="grid-company-name"
            >
              Company<span
                :class="{ 'text-red-500': !data.form.company.length }"
              >
                &nbsp;*</span
              >
            </label>
            <input
              class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
              id="grid-company-name"
              type="text"
              placeholder="Doe"
              v-model="data.form.company"
              :class="{
                'border-red-500': !data.form.company.length && data.submission,
              }"
            />
            <p
              v-if="!data.form.company.length && data.submission"
              class="text-red-500 text-xs italic"
            >
              Please fill out this field.
            </p>
          </div>

          <div class="w-full md:w-full mb-6 px-3">
            <label
              class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
              for="grid-email"
            >
              Email<span
                :class="{
                  'text-red-500':
                    !data.form.email.length || !isValidEmail(data.form.email),
                }"
              >
                &nbsp;*</span
              >
            </label>
            <input
              class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
              id="grid-email"
              type="email"
              placeholder="example@solarvista.com"
              v-model="data.form.email"
              :class="{
                'border-red-500': !data.form.email.length && data.submission,
              }"
            />
            <p
              v-if="!data.form.email.length && data.submission"
              class="text-red-500 text-xs italic"
            >
              Please enter a valid email.
            </p>
          </div>

          <div class="w-full md:w-full px-3 mb-6">
            <label
              class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
              for="grid-message"
            >
              Message<span
                :class="{ 'text-red-500': !data.form.message.length }"
              >
                &nbsp;*</span
              >
            </label>
            <textarea
              class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
              id="grid-message"
              type="text"
              placeholder="Type message here..."
              rows="5"
              :class="{
                'border-red-500': !data.form.message.length && data.submission,
              }"
              v-model="data.form.message"
            />
            <p
              v-if="!data.form.message.length && data.submission"
              class="text-red-500 text-xs italic"
            >
              Please fill out this field.
            </p>
          </div>
          <div class="w-full px-3 mt-6">
            <button
              type="submit"
              @click="(e) => submitForm(e)"
              class="btn block bg-[--primary] text-white rounded-md p-3 w-full mb-6 m-auto md:m-0 text-center"
            >
              <span v-if="!data.form.submitted && !data.loading">Send</span>
              <span v-else-if="data.form.submitted"> Sent</span>
              <ButtonSpinner v-else-if="data.loading" />
            </button>

            <p v-if="data.form.submitted" class="text-sm italic">
              Thank you {{ data.form.name }} for contacting Solar Vista. We'll
              be in contact 🌞
            </p>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped lang="scss">
// .content {
//   background-image: url("https://cdn.discordapp.com/attachments/1119779286174408716/1130059223594709002/rayn3r_a_company_meeting_for_a_solar_panel_company_hosted_at_a_b30e7ba9-90aa-42d7-b45c-891ed5e07849.png");
//   height: 100%;
// }
</style>