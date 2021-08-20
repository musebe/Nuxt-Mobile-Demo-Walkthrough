<template>
  <div>
    <Form
      @InstructorVideoUploaded="instructorVideoUploaded"
      @MobileAppVideoUploaded="mobileAppVideoUploaded"
    />
    <cld-video
      public-id="nuxtjs-mobile-demo-walkthrough-generator/iphone_se_template"
      width="1000"
      crop="scale"
      quality="auto"
      controls="true"
      autoplay="true"
      class="m-auto"
      v-if="mobileAppVideo && instructorVideo"
    >
      <cld-transformation
        :overlay="`video:${mobileAppVideo}`"
        width="405"
        height="750"
        gravity="center"
        x="-385"
        y="0"
        audioCodec="none"
      />
      <cld-transformation
        :overlay="`video:${instructorVideo}`"
        width="300"
        height="300"
        gravity="south_east"
        radius="150"
        x="100"
        y="100"
        crop="fill"
      />
    </cld-video>
  </div>
</template>

<script>
export default {
  data() {
    return {
      instructorVideo: null,
      mobileAppVideo: null,
    };
  },
  methods: {
    instructorVideoUploaded(public_id) {
      this.instructorVideo = public_id.replace("/", ":");
    },
    mobileAppVideoUploaded(public_id) {
      this.mobileAppVideo = public_id.replace("/", ":");
    },
  },
};
</script>
