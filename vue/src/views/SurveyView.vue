<template>
  <PageComponent>
    <template v-slot:header>
      <div class="flex items-center justify-between">
        <h1 class="text-3xl font-bold text-gray-900">
          {{ route.params.id ? model.title : "Create a Survey" }}
        </h1>
        <button
          @click="deleteSurvey()"
          v-if="route.params.id && !surveyLoading"
          type="button"
          class="py-2 px-3 text-white bg-red-500 rounded-md hover:bg-red-600"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5 -mt-1 inline-block"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
            />
          </svg>
          Delete Survey
        </button>
      </div>
    </template>

    <div v-if="surveyLoading" class="text-center">
      <svg  class="inline mr-2 w-8 h-8 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"></path>
        <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"></path>
      </svg>
    </div>

    <form v-else @submit.prevent="saveSurvey" class="animate-fade-in-down">
      <div class="shadow sm:rounded-md sm:overflow-hidden">
        <!--Survey fields-->
        <div class="px-4 py-5 bg-white space-y-6 sm:p-6">
          <!--Image-->
          <div>
            <label class="block text-base font-medium text-gray-700">
              Image
            </label>
            <div class="mt-1 flex flex-wrap items-center">
              <img
                v-if="model.image_url"
                :src="model.image_url"
                :alt="model.title"
                class="w-64 h-48 object-cover mr-5 mb-3"
              />
              <span
                v-else
                class="flex items-center justify-center h-12 w-12 rounded-full overflow-hidden bg-gray-100"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-[80%] w-[80%] text-gray-300"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
                  />
                </svg>
              </span>
              <button
                type="button"
                class="relative overflow-hidden bg-white py-2 px-3 border border-gray-300 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
              >
                <input
                  type="file"
                  @change="onImageChoose"
                  class="absolute left-0 right-0 top-0 bottom-0 opacity-0 cursor-pointer"
                />
                Change
              </button>
            </div>
          </div>

          <!--Title-->
          <div>
            <label for="title" class="block text-base font-medium text-gray-700"
              >Title</label
            >
            <input
              type="text"
              name="title"
              id="title"
              v-model="model.title"
              autocomplete="survey_title"
              class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
            />
          </div>
          <!--/ Title-->

          <!--Description-->
          <div>
            <label class="block text-base font-medium text-gray-700">
              Description
            </label>
            <div class="mt-1">
              <textarea
                name="description"
                id="description"
                rows="3"
                v-model="model.description"
                autocomplete="survey_description"
                placeholder="Describe your survey"
                class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 mt-1 block w-full sm:text-sm border border-gray-300 rounded-md"
              ></textarea>
            </div>
          </div>
          <!--/ Description-->

          <!--Expire Date-->
          <div>
            <label
              for="expire_date"
              class="block text-base font-medium text-gray-700"
              >Expire Date</label
            >
            <input
              type="date"
              name="expire_date"
              id="expire_date"
              v-model="model.expire_date"
              class="mt-1 focus:border-indigo-500 focus:border-indigo-500 block w-40 shadow-base sm:text-sm border-gray-300 rounded-md"
            />
          </div>
          <!-- / Expire Date-->

          <!-- Status -->
          <div class="flex items-center justify-start">
            <div class="flex items-center h-5">
              <input
                id="status"
                name="status"
                type="checkbox"
                v-model="model.status"
                class="focus:ring-indigo-500 h-4 w-4 text-indigo-600 border-gray-300 rounded"
              />
            </div>
            <div class="ml-3 text-base">
              <label for="status" class="font-medium text-gray-700"
                >Active</label
              >
            </div>
          </div>
          <!--/ Status -->

          <div class="px-4 py-5 bg-white space-y-6 sm:p-6">
            <h3
              class="text-2xl font-semibold flex items-center justify-between"
            >
              Questions

              <!-- Add new question -->
<!--              <button-->
<!--                type="button"-->
<!--                @click="addQuestion()"-->
<!--                class="flex items-center text-sm py-1 px-4 rounded-sm text-white bg-gray-600 hover:bg-gray-700"-->
<!--              >-->
<!--                <svg-->
<!--                  xmlns="http://www.w3.org/2000/svg"-->
<!--                  class="h-4 w-4"-->
<!--                  viewBox="0 0 20 20"-->
<!--                  fill="currentColor"-->
<!--                >-->
<!--                  <path-->
<!--                    fill-rule="evenodd"-->
<!--                    d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z"-->
<!--                    clip-rule="evenodd"-->
<!--                  />-->
<!--                </svg>-->
<!--                Add Question-->
<!--              </button>-->
              <!--/ Add new question -->
            </h3>
            <div
              v-if="!model.questions.length"
              class="text-center text-gray-600"
            >
              You don't have any questions created
            </div>
            <div
              v-for="(question, index) in model.questions"
              :key="question.id"
            >
              <QuestionEditor
                :question="question"
                :index="index"
                @change="changeQuestion"
                @addQuestion="addQuestion"
                @deleteQuestion="deleteQuestion"
              />
            </div>
          </div>
          <!-- Add new question -->
          <div class="flex items-center justify-center" style="margin-top: 0">
            <button
              type="button"
              @click="addQuestion()"
              class="flex items-center text-xl py-1 px-4 rounded-md text-white bg-gray-600 hover:bg-gray-700"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-6 w-6"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z"
                  clip-rule="evenodd"
                />
              </svg>
              Add Question
            </button>
          </div>
          <!--/ Add new question -->
          <!--Button-->
          <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
            <button
              :disabled="loading"
              type="submit"
              class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >

              <svg
                v-if="loading"
                class="animate-spin -ml-1 mr-3 h-5 w-5 text-white"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
              >
                <circle
                  class="opacity-25"
                  cx="12"
                  cy="12"
                  r="10"
                  stroke="currentColor"
                  stroke-width="4"
                ></circle>
                <path
                  class="opacity-75"
                  fill="currentColor"
                  d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
                ></path>
              </svg>
              Save
            </button>
          </div>
        </div>
      </div>
    </form>
  </PageComponent>
</template>

<script setup>
import { v4 as uuidv4 } from "uuid";
import store from "../store";
import { useRoute, useRouter } from "vue-router";
import { computed, ref, watch } from "vue";

import PageComponent from "../components/PageComponent.vue";
import QuestionEditor from "../components/editor/QuestionEditor.vue";

const route = useRoute();
const router = useRouter();

const surveyLoading = computed(() => store.state.survey.currentSurvey.loading);

let loading = ref(false);

let model = ref({
  title: "",
  status: false,
  description: null,
  image_url: null,
  expire_date: null,
  questions: [],
});

// Watch to current survey data change and when this happens we update local model
watch(
  // whenever this changes
  () => store.state.survey.currentSurvey.data,
  (newVal, oldVal) => {
    model.value = {
      ...JSON.parse(JSON.stringify(newVal)),
      status: !!newVal.status,
    };
  }
);

if (route.params.id) {
  store.dispatch("survey/getSurvey", route.params.id)
    .catch(err => {
      router.push({name: 'Page404'})
    });
}

function onImageChoose(ev) {
  const file = ev.target.files[0];

  const reader = new FileReader();
  reader.onload = () => {
    // The field to send on backend and apply validations
    model.value.image = reader.result;

    //The field to display here
    model.value.image_url = reader.result;
  };
  reader.readAsDataURL(file);
}

function addQuestion(index) {
  const newQuestion = {
    id: uuidv4(),
    type: "text",
    description: null,
    data: {},
  };
  model.value.questions.push(newQuestion);
  // model.value.questions.splice(index, 0, newQuestion);
}

function deleteQuestion(question) {
  model.value.questions = model.value.questions.filter((q) => q !== question);
}

function changeQuestion(question) {
  model.value.questions = model.value.questions.map((q) => {
    if (q.id === question.id) {
      return JSON.parse(JSON.stringify(question));
    }
    return q;
  });
}

function saveSurvey() {
  loading.value = true;
  store.dispatch("survey/saveSurvey", model.value).then(({ data }) => {
    store.commit("survey/notify", {
      type: "success",
      message: "Survey was successfully updated",
    });
    loading.value = false;
    router.push({
      name: "SurveyView",
      params: { id: data.data.id },
    });
  });
}

function deleteSurvey() {
  if (confirm("Are you really want to delete this Survey?")) {
    store.dispatch("survey/deleteSurvey", model.value.id).then(() => {
      router.push({ name: "Surveys" });
    });
  }
}
</script>

<style scoped></style>
