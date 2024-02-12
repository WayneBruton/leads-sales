<template>
  <div>
    <q-page class="test" style="display: flex; flex-direction: column">
      <div class="row">
        <div class="col-1" style="background-color: #dedbc9"></div>
        <div class="col">
          <div
            class="test"
            style="display: flex; width: 100%; justify-content: center; opacity: 0.9"
          >
            <!-- src="~assets/Opportunity_Prop_Header.jpg" -->
            <img
              class="header_img1"
              src="~assets/lsales-ogo-1.png"
              style="width: 33%; height: 40%"
            />
          </div>

          <h4 class="welcome text-center" style="margin: 0px; font-weight: bold">
            Thank you for your interest in one of our developments!
          </h4>
          <div class="development_divs_all">
            <div class="development_divs">
              <img src="~assets/hv.png" style="width: 35%" />
              <img src="~assets/HV_Dev.jpg" style="width: 80%" />
              <span style="font-weight: bold; font-size: 120%"
                >HERON VIEW DEVELOPMENT</span
              >
              <span style="font-size: 150%">Langeberg Ridge</span>
            </div>
            <div class="development_divs">
              <img src="~assets/hf.png" style="width: 35%" />
              <img src="~assets/HF_Dev.jpg" style="width: 80%" />
              <span style="font-weight: bold; font-size: 120%"
                >HERON FIELDS DEVELOPMENT</span
              >
              <span style="font-size: 150%">Langeberg Ridge</span>
            </div>
            <div class="development_divs">
              <img src="~assets/EN.png" style="width: 20%" />
              <img src="~assets/EN_Dev.jpg" style="width: 80%" />
              <span style="font-weight: bold; font-size: 120%">ENDULINI DEVELOPMENT</span>
              <span style="font-size: 150%">Protea Heights</span>
            </div>
          </div>
          <div style="margin: 0px 50px">
            <h5 class="text-center">
              ALL COSTS INCLUDED: (TRANSFER & BOND REGISTRATION COSTS)
            </h5>

            <hr />
          </div>

          <div class="footer-main">
            <div class="footer">
              <span class="footer-text"
                >Visit our website for more information on these developments or give us a
                call:</span
              >
            </div>
            <div class="footer">
              <div class="footer-content-left">
                <div class="footer-contact">
                  <q-icon name="computer" size="50px" />
                  <span
                    class="footer-text"
                    style="color: blue; cursor: pointer; text-decoration: underline"
                    ><a href="http://www.opportunityprop.co.za"
                      >www.opportunityprop.co.za</a
                    ></span
                  >
                </div>
                <div class="footer-contact">
                  <q-icon name="smartphone" size="50px" />
                  <span class="footer-text">Mobile: (066) 057 2220</span>
                </div>
                <div class="footer-contact">
                  <q-icon name="local_phone" size="50px" />
                  <span class="footer-text">Office: (021) 300 0586</span>
                </div>
                <div class="footer-contact">
                  <q-icon name="email" size="50px" />
                  <span class="footer-text"
                    >Email:
                    <span style="color: blue; cursor: pointer"
                      ><a href="mailTo: info@opportunityprop.co.za"
                        >info@opportunityprop.co.za</a
                      ></span
                    ></span
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-1" style="background-color: #dedbc9"></div>
      </div>
      <q-dialog v-model="card" persistent>
        <q-card
          class="my-card"
          style="width: 100vw; border: none; box-shadow: none; margin: 0px; padding: 0px"
        >
          <q-img src="~assets/lsales-ogo-1.png" style="height: 80%" />
          <q-card-section class="q-pt-none">
            <div class="text-center">
              <strong>Please fill in your details below:</strong>
            </div>
          </q-card-section>

          <q-card-section>
            <q-input
              outlined
              v-model="name"
              label="Name"
              style="margin: 5px; color: grey"
            />
            <q-input outlined v-model="surname" label="Surname" style="margin: 5px" />
            <q-input
              outlined
              v-model="email"
              label="email"
              style="margin: 5px"
              type="email"
            />
            <q-input
              outlined
              v-model="contact"
              label="Contact Number"
              style="margin: 5px"
              mask="(###) ### ####"
              hint="Please do not include contry code. Start with 0##."
              type="tel"
            />
            <q-select
              outlined
              v-model="development"
              :options="options"
              label="Which development are you interested in?"
              style="margin: 5px"
            />
            <div class="q-gutter-sm questions">
              <div style="margin-top: 20px"><strong>Contact me.</strong></div>
              <div>
                <q-radio
                  dark
                  v-model="contact_time"
                  keep-color
                  color="black"
                  val="ASAP"
                  label="ASAP"
                />
                <q-radio
                  dark
                  v-model="contact_time"
                  keep-color
                  val="After 5pm."
                  label="After 5pm."
                  color="black"
                />
              </div>
            </div>
          </q-card-section>
          <q-separator />
          <q-card-actions align="right">
            <q-btn flat color="black" label="Reset" @click="reset" />
            <q-space />
            <q-btn flat color="black" label="Submit" @click="submit" />
          </q-card-actions>
        </q-card>
      </q-dialog>
    </q-page>
  </div>
</template>

<script setup>
import { ref, onMounted, computed, onBeforeMount } from "vue";
// import { useUserStore } from "../stores/userStore";
import { useQuasar, QSpinnerClock } from "quasar";
import dayjs from "dayjs";
// import { useMeta } from "quasar";
import axios from "axios";
import { useRouter, useRoute } from "vue-router";

const $q = useQuasar();
const router = useRouter();
const route = useRoute();

let url = ref("");
if (process.env.DEV) {
  url.value = "http://127.0.0.1:8000";
} else {
  url.value = "https://omh-python.herokuapp.com";
}

const options = ["Endulini", "Heron Fields", "Heron View", "All of the Above"];

onMounted(() => {
  // Facebook Pixel script
  !(function (f, b, e, v, n, t, s) {
    if (f.fbq) return;
    n = f.fbq = function () {
      n.callMethod ? n.callMethod.apply(n, arguments) : n.queue.push(arguments);
    };
    if (!f._fbq) f._fbq = n;
    n.push = n;
    n.loaded = !0;
    n.version = "2.0";
    n.queue = [];
    t = b.createElement(e);
    t.async = !0;
    t.src = v;
    s = b.getElementsByTagName(e)[0];
    s.parentNode.insertBefore(t, s);
  })(window, document, "script", "https://connect.facebook.net/en_US/fbevents.js");
  fbq("init", "1433806687398157");
  fbq("track", "PageView");
  // console.log("Facebook Pixel script loaded");
});

// const position = ref("left");
// onMounted(() => {
//   console.log(route);
//   if (route.query == {}) {
//     // redirect back to where they came from
//     reset();
//     card.value = false;
//   }
// });
// const metaData = {
//   // sets document title
//   title: "Opportunity",
//   // optional; sets final title as "Index Page - My Website", useful for multiple level meta
//   // titleTemplate: (title) => `${title} - My Website`,
// };

const card = ref(false);

const open = () => {
  // position.value = "bottom";
  if (route.query.source) {
    card.value = true;
  } else {
    card.value = false;
  }
};

// Object.keys(route.params).length > 0;

const name = ref("");
const surname = ref("");
const email = ref("");
const contact = ref("");
const development = ref("");
const contact_time = ref("ASAP");

const reset = () => {
  name.value = "";
  surname.value = "";
  email.value = "";
  contact.value = "";
  development.value = "";
  contact_time.value = "ASAP";
};

// const sales_people = ref(["Morne", "Minette", "Yvette"]);

const submit = async () => {
  if (route.query == {}) {
    // redirect back to where they came from
    reset();
    card.value = false;
    return;
  }
  if (
    name.value == "" ||
    surname.value == "" ||
    email.value == "" ||
    contact.value == "" ||
    development.value == ""
  ) {
    $q.notify({
      message: `Please fill in all the fields.`,
      color: "red",
      position: "center",
      icon: "warning",
      timeout: 2000,
    });
    return;
  }
  $q.loading.show({
    spinner: QSpinnerClock,
    spinnerSize: 100,
    spinnerColor: "black",
    message: "Submitting your details...",
  });
  let submission_date = dayjs().format("YYYY-MM-DD HH:mm:ss");
  await axios
    .post(`${url.value}/post_sales_lead_form`, {
      name: name.value,
      surname: surname.value,
      email: email.value,
      contact: contact.value,
      development: development.value,
      source: route.query,
      submission_date: submission_date,
      contact_time: contact_time.value,
      message: "",
    })
    .then((response) => {
      // let sales_person =
      //   sales_people.value[Math.floor(Math.random() * sales_people.value.length)];
      console.log(response.data);
      let sales_person = response.data.sales_person;
      $q.notify({
        message: `Thank you for your interest in our development! ${sales_person} will be in contact with you shortly.`,
        color: "green",
        position: "center",
        icon: "check",
        timeout: 2000,
      });
      reset();
      $q.loading.hide();
      card.value = false;
    })
    .catch((error) => {
      card.value = false;
      $q.loading.hide();
      $q.notify({
        message: `Something went wrong, please try again.`,
        color: "red",
        position: "center",
        icon: "warning",
        timeout: 2000,
      });
    });

  // choose a sales person randomly

  // router.back();
};

open();
</script>

<style scoped>
.test {
  background-color: #f3f2ec;
  margin: 0;
  padding: 0;
}
.development_divs_all {
  display: flex;
  width: 83vw;
  margin: 10px;
  justify-content: space-around;
}
.development_divs {
  display: flex;
  flex-direction: column;
  max-width: 30%;
  align-items: center;
  justify-content: center;
}

.footer-main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  /* margin-top: 50px; */
}

.footer {
  display: flex;
  /* flex-direction: column; */
  /* align-items: center; */
  justify-content: space-between;
  /* margin-top: 50px; */
}
.footer-text {
  font-size: 150%;
  font-weight: bold;
  margin-top: 8px;
  width: 100%;
  /* margin: 0px 50px; */
}
.footer-content-left {
  display: flex;
  flex-direction: column;
  /* align-items: center; */
  justify-content: space-between;
  width: 100%;
}

.footer-contact {
  display: flex;
  width: 100%;
  /* align-items: center; */
  justify-content: space-between;
  /* margin: 10px; */
}

.questions {
  display: flex;
  justify-content: space-between;
}

@media screen and (max-width: 600px) {
  .development_divs_all {
    flex-direction: column;
  }
  .development_divs {
    max-width: 100%;
    margin-top: 50px;
  }
  .welcome {
    font-size: 150%;
  }
  .footer-text {
    font-size: 100%;
    font-weight: bold;
    margin-top: 8px;
    width: 100%;
    /* margin: 0px 50px; */
  }
  .footer {
    margin-top: 50px;
  }
  .questions {
    flex-direction: column;
    align-items: center;
    margin-top: 10px;
  }
}

@keyframes example {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* .my-card {
  animation-name: example;
  animation-duration: 2.5s;
} */
</style>
