<template>
  <EmptyAlert
    message="There's no video here!"
    :messageBold="boldAlertMessage(user.role.name)"
    v-if="!classVideos[0]"
  />

  <div
    v-for="classVideo in classVideos"
    :key="classVideo.id"
    class="bg-white rounded-md shadow-sm px-8 py-6 mb-3 flex items-center justify-between"
  >
    <div class="w-1/2 sm:w-1/3 pr-10 flex items-center">
      <div class="p-2 rounded-full bg-gradient-to-br from-red-500 to-red-300">
        <svg
          class="fill-current w-5 h-5 text-white"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
          fill-rule="evenodd"
          clip-rule="evenodd"
        >
          <path
            d="M22 14.236v3.528l-2-1v-1.528l2-1zm2-3.236l-6 3v4l6 3v-10zm-10 2v5.5c0 .276-.224.5-.5.5h-9c-.276 0-.5-.224-.5-.5v-5.5c-.702 0-1.373-.127-2-.35v6.35c0 1.104.896 2 2 2h10c1.104 0 2-.896 2-2v-6.35c-.627.223-1.298.35-2 .35zm0-8c1.103 0 2 .897 2 2s-.897 2-2 2-2-.897-2-2 .897-2 2-2zm-10 0c1.103 0 2 .897 2 2s-.897 2-2 2-2-.897-2-2 .897-2 2-2zm10-2c-2.209 0-4 1.791-4 4s1.791 4 4 4 4-1.791 4-4-1.791-4-4-4zm-10 0c-2.209 0-4 1.791-4 4s1.791 4 4 4 4-1.791 4-4-1.791-4-4-4z"
          />
        </svg>
      </div>

      <div class="ml-5 flex flex-col w-full">
        <h3 class="truncate font-semibold text-sm">
          {{ classVideo.title }}
        </h3>
        <p class="mt-1 text-sm">Posted at Nov 13</p>
      </div>
    </div>

    <div class="flex">
      <button
        @click="
          $router.push({
            name: 'LearningVideoDetail',
            params: {
              classroomId: $route.params.classroomId,
              learningVideoId: classVideo.id,
            },
          })
        "
        class="flex items-center border border-red-500 text-red-500 px-4 py-1 mr-6 text-sm rounded-full transition duration-100 ease-in-out hover:bg-red-500 hover:border-red-500 hover:text-white focus:outline-none"
      >
        <svg
          class="fill-current w-4 h-4"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
          fill-rule="evenodd"
          clip-rule="evenodd"
        >
          <path
            d="M23.111 20.058l-4.977-4.977c.965-1.52 1.523-3.322 1.523-5.251 0-5.42-4.409-9.83-9.829-9.83-5.42 0-9.828 4.41-9.828 9.83s4.408 9.83 9.829 9.83c1.834 0 3.552-.505 5.022-1.383l5.021 5.021c2.144 2.141 5.384-1.096 3.239-3.24zm-20.064-10.228c0-3.739 3.043-6.782 6.782-6.782s6.782 3.042 6.782 6.782-3.043 6.782-6.782 6.782-6.782-3.043-6.782-6.782zm2.01-1.764c1.984-4.599 8.664-4.066 9.922.749-2.534-2.974-6.993-3.294-9.922-.749z"
          />
        </svg>
        <span class="inline-block ml-2">Detail</span>
      </button>

      <div class="relative" v-if="user.role.name == 'Teacher'">
        <button
          @click.prevent="
            classVideo.isSettingModalActive = !classVideo.isSettingModalActive
          "
          class="ring-red-400 ring-opacity-50 focus:outline-none focus:ring focus:ring-offset-4 rounded"
        >
          <svg
            class="fill-current w-4 h-4"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M12 18c1.657 0 3 1.343 3 3s-1.343 3-3 3-3-1.343-3-3 1.343-3 3-3zm0-9c1.657 0 3 1.343 3 3s-1.343 3-3 3-3-1.343-3-3 1.343-3 3-3zm0-9c1.657 0 3 1.343 3 3s-1.343 3-3 3-3-1.343-3-3 1.343-3 3-3z"
            />
          </svg>
        </button>

        <div
          v-if="classVideo.isSettingModalActive"
          class="absolute top-6 right-0 w-40 bg-white shadow-md border rounded-md py-1 z-50"
        >
          <button
            @click="
              classVideo.isEditModalActive = !classVideo.isEditModalActive;
              classVideo.isSettingModalActive = !classVideo.isSettingModalActive;
            "
            class="w-full space-x-3 text-black-300 font-medium px-4 py-2 flex items-center hover:bg-gray-100 focus:outline-none"
          >
            <svg
              class="fill-current w-4 h-4"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
            >
              <path
                d="M18.363 8.464l1.433 1.431-12.67 12.669-7.125 1.436 1.439-7.127 12.665-12.668 1.431 1.431-12.255 12.224-.726 3.584 3.584-.723 12.224-12.257zm-.056-8.464l-2.815 2.817 5.691 5.692 2.817-2.821-5.693-5.688zm-12.318 18.718l11.313-11.316-.705-.707-11.313 11.314.705.709z"
              />
            </svg>

            <div class="inline-block text-sm">Edit</div>
          </button>

          <button
            @click="
              classVideo.isDeleteModalActive = !classVideo.isDeleteModalActive;
              classVideo.isSettingModalActive = !classVideo.isSettingModalActive;
            "
            class="w-full space-x-3 text-black-300 font-medium px-4 py-2 flex items-center hover:bg-gray-100 focus:outline-none"
          >
            <svg
              class="fill-current w-4 h-4 stroke-current stroke-1"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
              fill-rule="evenodd"
              clip-rule="evenodd"
            >
              <path
                d="M12 11.293l10.293-10.293.707.707-10.293 10.293 10.293 10.293-.707.707-10.293-10.293-10.293 10.293-.707-.707 10.293-10.293-10.293-10.293.707-.707 10.293 10.293z"
              />
            </svg>

            <div class="inline-block text-sm">Delete</div>
          </button>
        </div>
      </div>
    </div>

    <transition
      id="dark-bg"
      enter-active-class="animate__animated animate__fadeIn animate__faster"
      leave-active-class="animate__animated animate__fadeOut animate__faster"
    >
      <div
        @click.prevent="
          classVideo.isDeleteModalActive = false;
          classVideo.isSettingModalActive = false;
          classVideo.isEditModalActive = false;
        "
        v-if="
          classVideo.isDeleteModalActive ||
          classVideo.isSettingModalActive ||
          classVideo.isEditModalActive
        "
        class="fixed inset-0 transition-opacity"
        aria-hidden="true"
      >
        <div class="absolute inset-0 bg-gray-500 opacity-50"></div>
      </div>
    </transition>

    <transition
      id="delete-modal"
      enter-active-class="animate__animated animate__zoomInDown"
      leave-active-class="animate__animated animate__zoomOut"
    >
      <div
        class="fixed z-10 inset-0 overflow-y-auto"
        v-if="classVideo.isDeleteModalActive"
      >
        <div
          class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
        >
          <span
            class="hidden sm:inline-block sm:align-middle sm:h-screen"
            aria-hidden="true"
            >&#8203;</span
          >

          <div
            class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full"
            role="dialog"
            aria-modal="true"
            aria-labelledby="modal-headline"
          >
            <div class="bg-gray-50 py-5">
              <img
                src="./../assets/logo.svg"
                alt="larns logo"
                class="w-24 px-6"
              />
            </div>

            <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
              <div class="sm:flex sm:items-start">
                <div
                  class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10"
                >
                  <svg
                    class="h-6 w-6 text-red-600"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                    aria-hidden="true"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
                    />
                  </svg>
                </div>
                <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                  <h3
                    class="text-lg leading-6 font-medium text-gray-900"
                    id="modal-headline"
                  >
                    {{ classVideo.title }}
                  </h3>
                  <div class="mt-2">
                    <p class="text-sm text-gray-500">
                      Are you sure you want to delete?
                    </p>
                  </div>
                </div>
              </div>
            </div>
            <div
              class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse"
            >
              <button
                @click="submitDeleteClassVideo(classVideo.id)"
                type="submit"
                class="w-full inline-flex justify-center items-center rounded-full border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm"
              >
                <CircleLoading v-if="isDeleteButtonClicked" />

                Delete
              </button>

              <button
                @click="
                  classVideo.isDeleteModalActive = !classVideo.isDeleteModalActive
                "
                type="button"
                class="mt-3 w-full inline-flex justify-center rounded-full border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-opacity-75 focus:ring-red-400 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
              >
                Cancel
              </button>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <transition id="edit-modal" name="modal-create">
      <form
        autocomplete="off"
        @submit.prevent="submitEditClassVideo(classVideo)"
        v-if="classVideo.isEditModalActive"
        class="fixed rounded-xl w-120 bg-white flex flex-col z-50 top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2"
      >
        <div class="bg-gray-50 py-6 rounded-tl-xl rounded-tr-xl">
          <img src="./../assets/logo.svg" alt="larns logo" class="w-24 px-6" />
        </div>

        <h2 class="text-2xl font-bold px-6 mt-2">Edit video</h2>

        <div class="mt-6 px-6 pb-3">
          <div class="flex flex-col mb-6">
            <label
              for="title"
              class="text-xs font-bold tracking-wide uppercase"
            >
              title
            </label>
            <input
              v-model="classVideo.title"
              type="text"
              id="title"
              class="appearance-none w-full bg-transparent border-b border-gray-700 px-1 py-2 focus:border-red-400 focus:outline-none"
            />
          </div>
          <div class="flex flex-col mb-6">
            <label
              for="description"
              class="text-xs font-bold tracking-wide uppercase"
            >
              Description
            </label>

            <textarea
              v-model="classVideo.description"
              id="description"
              class="appearance-none w-full bg-transparent border-b border-gray-700 px-1 py-2 focus:border-red-400 focus:outline-none"
            ></textarea>
          </div>
        </div>

        <div
          class="px-6 py-4 bg-gray-50 rounded-bl-xl rounded-br-xl flex justify-end space-x-2"
        >
          <button
            @click.prevent="classVideo.isEditModalActive = false"
            class="mt-3 w-full inline-flex justify-center rounded-full border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-opacity-75 focus:ring-red-300 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
          >
            <span class="text-sm font-bold">Cancel</span>
          </button>

          <button
            type="submit"
            class="inline-flex items-center self-end bg-red-400 hover:bg-red-500 text-white py-2 px-6 transition duration-100 ease-in border border-red-400 hover:border-red-500 rounded-full focus:outline-none"
          >
            <CircleLoading v-if="isEditButtonClicked" />

            <span class="text-sm font-bold">Edit</span>
          </button>
        </div>
      </form>
    </transition>
  </div>
</template>

<script>
import { ref } from "vue";
import { useRoute } from "vue-router";
import useClassVideos from "../modules/class-videos";
import EmptyAlert from "./EmptyAlert.vue";
import CircleLoading from "./CircleLoading.vue";
import useAuth from "../modules/auth";

export default {
  components: { EmptyAlert, CircleLoading },
  name: "LearningVideosList",
  async setup() {
    const route = useRoute();

    const isEditButtonClicked = ref(false);
    const isDeleteButtonClicked = ref(false);

    const user = ref(null);
    const { authUser } = useAuth();

    user.value = await authUser();

    const { load, classVideos, destroy, update } = useClassVideos();

    await load(route.params.classroomId);

    function boldAlertMessage(roleName) {
      if (roleName == "Teacher") {
        return "Go upload one!";
      } else {
        return "Horayy!";
      }
    }

    async function submitDeleteClassVideo(id) {
      isDeleteButtonClicked.value = true;

      await destroy(route.params.classroomId, id);

      isDeleteButtonClicked.value = false;
    }

    async function submitEditClassVideo(classVideo) {
      isEditButtonClicked.value = true;

      await update(route.params.classroomId, classVideo.id, classVideo);

      isEditButtonClicked.value = false;
    }

    return {
      classVideos,
      user,
      isEditButtonClicked,
      isDeleteButtonClicked,
      submitDeleteClassVideo,
      submitEditClassVideo,
      boldAlertMessage,
    };
  },
};
</script>

<style>
</style>