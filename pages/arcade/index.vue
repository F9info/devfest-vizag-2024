<template>
  <NuxtLayout name="default">
    <v-container fluid class="mt-5">
      <v-row>
        <v-col>
          <h1>Arcade</h1>
          <p>Felt something missing? Well, you are at the right spot. Grab your thinking hats and put
        them on as we're going to play and compete to learn and earn, all while having fun.</p>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="6" md="4" v-for="(item, index) in arcadeData" :key="index">
         <div style="border: 2px solid black; display: flex; flex-direction: column; align-items: center; border-radius: 1rem; overflow: hidden">
          <img
           :src="'img/arcade/' + item.image"
            height="160"
            style="width: 100%; object-fit: cover;"
          />
           <div style="padding: 20px; width: 100%">
            <h3 class="font-medium">{{item.name}}</h3>
            <p style="font-size: 12px; opacity: 50%;">
              {{ item.difficulty }} | {{ item.category }}
            </p>
            <div style="margin: 4px 0px 6px;">
       
          <div v-for="author in item.author"
            style="display: inline-flex; margin:3px; padding: 4px 6px; border: 1px #202023 solid; border-radius: 80px; column-gap: 6px; align-items:center; width: fit-content;">
            <img :src="'../img/team/' + author.image"
              style="width: 18px; height: 18px; object-fit:cover; border-radius: 20px;" />
            <p class="mr-2" style="font-size: 14px; font-weight: 600;">{{author.name}}</p>
          </div>
          
      </div>
            <p style="font-size: 16px;">{{ item.description }}
            </p>
            
            
            <v-btn :href="item.link" :disabled="item.available != true" style="border: 1px #202024 solid; border-radius: 48px; font-size: 14px; font-weight: 600" class="mt-3 px-5 py-1">
                {{ item.available == false ? 'Coming Soon' : 'Play Now' }}
            </v-btn>
          </div>
        </div>
      </v-col>
      </v-row>
    </v-container>
  </NuxtLayout>
</template>


<script setup>
const { arcadeData, mainData } = useJSONData();

definePageMeta({
  layout: false,
});

useSeoMeta({
  contentType: "text/html; charset=utf-8",
  title: "Arcade - " + mainData.eventInfo.name + " | " + mainData.communityName,
  description: mainData.eventInfo.description.short,
  keywords: mainData.seo.keywords,
  ogLocale: 'en_US',
  author: "Manas Malla",
  creator: "Manas Malla",
  viewport: "width=device-width, initial-scale=1.0",
  ogTitle: "Arcade - " + mainData.eventInfo.name + " | " + mainData.communityName,
  ogDescription: mainData.eventInfo.description.short,
  ogImage: `${mainData.seo.hostUrl}/thumbnail.png?auto=format&fit=crop&frame=1&h=512&w=1024`,
  ogUrl: mainData.seo.hostUrl,
  ogType: "website",
  twitterTitle:
    "Arcade - " + mainData.eventInfo.name + " | " + mainData.communityName,
  twitterDescription: mainData.eventInfo.description.short,
  twitterImage: `${mainData.seo.hostUrl}thumbnail.png?auto=format&fit=crop&frame=1&h=512&w=1024`,
  twitterCard: "summary_large_image",
});
</script>

<style scoped></style>