<template>
  <div class="max-w-7xl mx-auto sm:p-6 lg:p-8">
    <div>
      <div class="md:grid md:grid-cols-3 md:gap-6">
        <div class="md:col-span-1">
          <div class="px-4 sm:px-0">
            <h3 class="text-lg font-medium leading-6 text-gray-900">
              Video uploads
            </h3>
            <p class="mt-1 text-sm text-gray-600">
              In order to generate the demo video, we need an upload of both the
              mobile app and the instructor videos.
            </p>
          </div>
        </div>
        <div class="mt-5 md:mt-0 md:col-span-2">
          <form action="#" method="POST" @submit.prevent="submit">
            <div class="shadow sm:rounded-md sm:overflow-hidden">
              <div class="px-4 py-5 bg-white space-y-6 sm:p-6">
                <div class="grid grid-cols-3 gap-6">
                  <div class="col-span-3 sm:col-span-2">
                    <label
                      for="mobile_app_video"
                      class="block text-sm font-medium text-gray-700"
                    >
                      Mobile app video
                    </label>
                    <div class="mt-5 flex rounded-md shadow-sm">
                      <input
                        type="file"
                        name="mobile_app_video"
                        id="mobile_app_video"
                        required
                        class="
                          focus:ring-indigo-500
                          focus:border-indigo-500
                          flex-1
                          block
                          w-full
                          rounded-none rounded-r-md
                          sm:text-sm
                          border-gray-300
                        "
                      />
                    </div>
                  </div>
                </div>
                <div class="grid grid-cols-3 gap-6">
                  <div class="col-span-3 sm:col-span-2">
                    <label
                      for="instructor_video"
                      class="block text-sm font-medium text-gray-700"
                    >
                      Instructor video
                    </label>
                    <div class="mt-5 flex rounded-md shadow-sm">
                      <input
                        type="file"
                        name="instructor_video"
                        id="instructor_video"
                        required
                        class="
                          focus:ring-indigo-500
                          focus:border-indigo-500
                          flex-1
                          block
                          w-full
                          rounded-none rounded-r-md
                          sm:text-sm
                          border-gray-300
                        "
                        placeholder="www.example.com"
                      />
                    </div>
                  </div>
                </div>
              </div>
              <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
                <button
                  type="submit"
                  class="
                    inline-flex
                    justify-center
                    py-2
                    px-4
                    border border-transparent
                    shadow-sm
                    text-sm
                    font-medium
                    rounded-md
                    text-white
                    bg-indigo-600
                    hover:bg-indigo-700
                    focus:outline-none
                    focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500
                  "
                >
                  Save
                </button>
                <p
                  class="my-5 text-sm"
                  v-if="uploadingMobileAppVideo || uploadingInstructorVideo"
                >
                  Upload in progress
                </p>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>

    <div class="hidden sm:block" aria-hidden="true">
      <div class="py-5">
        <div class="border-t border-gray-200" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uploadingMobileAppVideo: false,
      uploadingInstructorVideo: false,
    };
  },
  methods: {
    submit(e) {
      this.uploadMobileAppVideo(e);
      this.uploadInstructorVideo(e);
    },

    async uploadMobileAppVideo(e) {
      this.uploadingMobileAppVideo = true;

      // Mobile app video file
      let file = e.target.mobile_app_video.files[0];

      /* Read data */
      const data = await this.readData(file);

      /* upload the converted data */
      const resp = await this.$cloudinary.upload(data, {
        upload_preset: "default-preset",
        folder: `nuxtjs-mobile-demo-walkthrough-generator`,
      });

      this.$emit("MobileAppVideoUploaded", resp.public_id);

      this.uploadingMobileAppVideo = false;
    },

    async uploadInstructorVideo(e) {
      this.uploadingInstructorVideo = true;

      // Instructor video file
      let file = e.target.instructor_video.files[0];

      /* Read data */
      const data = await this.readData(file);

      /* upload the converted data */
      const resp = await this.$cloudinary.upload(data, {
        upload_preset: "default-preset",
        folder: `nuxtjs-mobile-demo-walkthrough-generator`,
      });

      this.$emit("InstructorVideoUploaded", resp.public_id);

      this.uploadingInstructorVideo = false;
    },
    readData(f) {
      return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onloadend = () => resolve(reader.result);
        reader.readAsDataURL(f);
      });
    },
  },
};
</script>