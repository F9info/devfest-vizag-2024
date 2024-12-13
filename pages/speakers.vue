<template>
  <NuxtLayout name="default">
    <v-container fluid class="mt-5">
      <v-row>
        <v-col md="12">
          <h1>Speakers</h1>
          <p>
            Our speakers are influential leaders and allies actively involved in
            various communities within their organizations, cities, countries,
            and beyond, making a significant impact through their contributions
            and support.
          </p>
        </v-col>
      </v-row>

      <v-row>
        <v-col md="2" cols="6" sm="3"
          v-for="(item, index) in speakersData.filter((speaker) => speaker.mentor === true && !speaker.expert)"
          :key="index">
          <common-speaker-card :data="item" @request-time="openBottomSheet"
            :can-request-time="!mentorRequests.includes(item.name)" />
        </v-col>

      </v-row>
    </v-container>

    <!-- Experts -->
    <v-container fluid v-if="(speakersData.filter((speaker) => speaker.expert === true)).length > 0">
      <v-row>
        <v-col md="12">
          <h1>Experts</h1>
          <p>
            Our experts are influential leaders and allies actively involved in
            various communities within their organizations, cities, countries,
            and beyond, making a significant impact through their contributions
            and support.
          </p>
        </v-col>
      </v-row>

      <v-row>
        <v-col md="2" cols="6" sm="3" v-for="(item, index) in speakersData.filter((expert) => expert.expert === true)"
          :key="index">
          <common-speaker-card :data="item" @request-time="openBottomSheet"
            :can-request-time="!mentorRequests.includes(item.name)" />
        </v-col>

      </v-row>
    </v-container>
    <CommonAskAPunditBottomSheet v-if="bottomSheetOpen" :professional="selectedProfessional"
      @close="bottomSheetOpen = false" :add-mentor-request="() => {
        mentorRequests.push(selectedProfessional.name);
      }" />
  </NuxtLayout>
</template>

<script setup>
import { query, where, collection, getDocs } from 'firebase/firestore';

const { mainData, speakersData } = useJSONData();
const selectedProfessional = ref(null);
const bottomSheetOpen = ref(false);

definePageMeta({
  layout: false,
});

const openBottomSheet = (professional) => {
  selectedProfessional.value = professional;
  bottomSheetOpen.value = true;
};

const mentorRequests = useState('requests', () => []);

const user = useCurrentUser();
const db = useFirestore();
watch(user, async (_) => {
  if (user.value) {
    console.log('User logged in');
    const q = query(collection(db, 'mentor-request'), where('uid', '==', user.value.uid));
    const snapshot = await getDocs(q);
    console.log(snapshot, user.value.uid);
    snapshot.forEach((doc) => {
      mentorRequests.value.push(doc.data().mentor);
    });
  }
});

useSeoMeta({
  contentType: "text/html; charset=utf-8",
  title:
    "Speakers - " + mainData.eventInfo.name + " | " + mainData.communityName,
  description: mainData.eventInfo.description.short,
  keywords: mainData.seo.keywords,
  ogLocale: 'en_US',
  author: "The Ananta Studio",
  creator: "The Ananta Studio",
  viewport: "width=device-width, initial-scale=1.0",
  ogTitle:
    "Speakers - " + mainData.eventInfo.name + " | " + mainData.communityName,
  ogDescription: mainData.eventInfo.description.short,
  ogImage: `${mainData.seo.hostUrl}/thumbnail.png?auto=format&fit=crop&frame=1&h=512&w=1024`,
  ogUrl: mainData.seo.hostUrl,
  ogType: "website",
  twitterTitle:
    "Speakers - " + mainData.eventInfo.name + " | " + mainData.communityName,
  twitterDescription: mainData.eventInfo.description.short,
  twitterImage: `${mainData.seo.hostUrl}thumbnail.png?auto=format&fit=crop&frame=1&h=512&w=1024`,
  twitterCard: "summary_large_image",
});
</script>

<style scoped></style>
