<template>

  <VideoBackground
      src="/vid5.mp4"
      style="height: 100vh;"
  >
    <div class="flex flex-row">


      <div class="flex flex-col lg:flex-row justify-center items-center bg-repeat w-full h-full absolute z-50 overflow-y-auto"
           style="background-image: url('/pa6.png')">
        <div class="flex flex-col backdrop-blur-md p-10 mt-72 lg:mt-0 rounded-xl border-2 items-center max-w-[400px] max-h-[390px]">
          <div>
            <a :href="'https://github.com/' + userData.login"> <img
                :src="userData.avatarUrl" alt="GitHub Avatar"
                class="rounded-full h-24 border-4 border-gray-white mb-2.5">
            </a>
          </div>
          <a :href="'https://github.com/' + userData.login"><h1 class="text-3xl font-black text-white hover:underline">
            {{
              userData.name
            }}</h1></a>

          <ul class="list-none">
            <li class="text-white flex mb-1 mt-2 text-pretty max-w-[256px] max-h-[72px] overflow-y-auto" style="">
              <IconInfo class="mr-2 min-w-6 min-h-6"/>
              {{ userData.bio }}
            </li>
            <li class="text-white flex mb-1">
              <IconLocation class="mr-2"/>
              {{ userData.location }}
            </li>
            <li class="text-white flex mb-1">
              <IconStack class="mr-2"/>
              Repos:
              {{ userData.public_repos }}
            </li>
          </ul>
        </div>

        <div class="flex flex-col">

          <div
              class="transition-all flex flex-col ml-0 mt-10 lg:ml-10 lg:mt-0 backdrop-blur-md hover:backdrop-blur-xl bg-white/50 lg:text-white lg:bg-transparent hover:bg-white/50 text-black rounded-none hover:text-black p-10 lg:rounded-t-xl lg:border-white border-transparent border-2 border-b-0 max-w-[700px] max-h-[300px] overflow-y-auto">

            <h3 class="font-black mb-4 text-3xl flex align-center justify-center">📰Articles</h3>
            <div  class="flex flex-col items-start justify-between">
              <ul v-for="post in posts" :key="post.id" class="list-none">
                <li  class="text-1xl font-bold hover:underline flex">

                  <button class="hover:underline" @click="selectPost(post)">{{ post.title }}</button>

                </li>

                <li class="text-1xl font-normal mb-4 flex mt-0.5">
                  <IconDate class=""/>
                  {{ formatTime(post.date) }}
                </li>
              </ul>
            </div>
          </div>

          <div
              class="transition-all flex flex-col ml-0 lg:ml-10  backdrop-blur-md hover:backdrop-blur-xl  bg-yellow-400/30 lg:bg-transparent hover:bg-yellow-400/30 p-10 lg:rounded-b-xl border-2 border-t-1 max-w-[700px] lg:border-white border-transparent rounded-none max-h-[300px] overflow-y-auto">
            <h3 class="font-black mb-4 text-3xl text-white flex align-center justify-center">📂Projects</h3>
            <ul  class="list-decimal">
              <li v-for="project in projects" :key="project.id" class="font-bold hover:underline text-white">
                <button class="hover:underline" @click="selectProject(project)"> {{ project.title }}</button>
              </li>


            </ul>
          </div>
        </div>

      </div>
    </div>
  </VideoBackground>
  <div v-if="selectedProject" :class="{ 'fullscreenbg': isFullScreen }" class="z-50 fixed inset-0 overflow-y-auto flex items-center justify-center">

    <!-- Этот элемент будет на весь экран -->
    <div class="flex items-center justify-center">

      <div :class="{ 'fullscreen': isFullScreen }"
           class="dark:bg-neutral-950 bg-white dark:text-white transition-all text-gray-800 lg:rounded-2xl rounded-none p-8 dark:border-2 border-4 lg:dark:border-white border-transparent border-blue-500 lg:h-2/4  h-screen w-full lg:w-2/4 overflow-y-auto relative">

        <!-- Dialog content goes here -->

        <div class="text-3xl w-full mb-4 flex items-center ">
          <div class="w-full">{{ selectedProject.title }}</div>
          <button class="flex flex-col justify-items-end items-end mr-2" @click="toggleFullScreen">
            <svg v-if="!isFullScreen" class="w-9 w-9" viewBox="0 0 19.5 19.5" xmlns="http://www.w3.org/2000/svg">
              <path d="M5.53,5.21a.32.32,0,0,0-.32.32v2.6a.33.33,0,0,1-.65,0V5.53a1,1,0,0,1,1-1h2.6a.33.33,0,0,1,0,.65Z"
                    style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M11.05,4.88a.32.32,0,0,1,.32-.32H14a1,1,0,0,1,1,1v2.6a.33.33,0,0,1-.65,0V5.53A.32.32,0,0,0,14,5.21h-2.6a.32.32,0,0,1-.32-.33"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M4.88,11.05a.32.32,0,0,1,.33.32V14a.32.32,0,0,0,.32.32h2.6a.33.33,0,0,1,0,.65H5.53a1,1,0,0,1-1-1h0v-2.6a.32.32,0,0,1,.32-.32"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M14.62,11.05a.32.32,0,0,1,.32.32V14a1,1,0,0,1-1,1h-2.6a.33.33,0,0,1,0-.65H14a.32.32,0,0,0,.32-.32v-2.6a.32.32,0,0,1,.33-.32"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <circle cx="9.75" cy="9.75" r="9"
                      style="fill:none;stroke:#fff;stroke-linecap:round;stroke-linejoin:round;stroke-width:1.5px"/>
            </svg>
            <svg v-if="isFullScreen" class="w-9 w-9" viewBox="0 0 19.5 19.5" xmlns="http://www.w3.org/2000/svg">
              <circle cx="9.75" cy="9.75" r="9"
                      style="fill:none;stroke:#fff;stroke-linecap:round;stroke-linejoin:round;stroke-width:1.5px"/>
              <path
                  d="M7.91,3.87a.37.37,0,0,1,.37.37V7.18a1.1,1.1,0,0,1-1.1,1.1H4.24a.37.37,0,1,1,0-.73H7.18a.38.38,0,0,0,.37-.37V4.24a.37.37,0,0,1,.36-.37"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M11.59,3.87a.37.37,0,0,1,.36.37V7.18a.38.38,0,0,0,.37.37h2.94a.37.37,0,1,1,0,.73H12.32a1.1,1.1,0,0,1-1.1-1.1V4.24a.37.37,0,0,1,.37-.37"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M3.87,11.59a.37.37,0,0,1,.37-.37H7.18a1.1,1.1,0,0,1,1.1,1.1v2.94a.37.37,0,1,1-.73,0V12.32A.38.38,0,0,0,7.18,12H4.24a.37.37,0,0,1-.37-.36"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M11.22,12.32a1.1,1.1,0,0,1,1.1-1.1h2.94a.37.37,0,1,1,0,.73H12.32a.38.38,0,0,0-.37.37v2.94a.37.37,0,1,1-.73,0Z"
                  style="stroke:#fff;stroke-miterlimit:10"/>
            </svg>
          </button>
          <button class="flex flex-col justify-items-end items-end" @click="closeCustomWindow">

            <svg class="w-11 h-11" fill="none" stroke="currentColor" stroke-width="1.5"
                 viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path d="m9.75 9.75 4.5 4.5m0-4.5-4.5 4.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" stroke-linecap="round"
                    stroke-linejoin="round"/>
            </svg>
          </button>

        </div>
        <hr>
        <div v-if="isFullScreen" class="mt-4" v-html="selectedProject.content">
        </div>
        <div v-if="!isFullScreen" >
          <h1 class="font-bold mt-4">Short description:</h1>
          <div class="mt-4" v-html="selectedProject.excerpt">

          </div>
        </div>
        <div v-if="!isFullScreen" class="mt-4">
          <button class="flex flex-col justify-items-end items-end mr-2 hover:underline" @click="toggleFullScreen">Learn more</button>
        </div>
        <!-- Button to close the dialog -->

      </div>
    </div>
  </div>
  <div v-if="selectedPost" :class="{ 'fullscreenbg': isFullScreen }" class="z-50 fixed inset-0 overflow-y-auto flex items-center justify-center">

    <!-- Этот элемент будет на весь экран -->
    <div class="flex items-center justify-center">

      <div :class="{ 'fullscreen': isFullScreen }"
           class="dark:bg-neutral-950 bg-white dark:text-white transition-all text-gray-800 lg:rounded-2xl rounded-none p-8 dark:border-2 border-4 lg:dark:border-white border-transparent border-blue-500 lg:h-2/4  h-screen w-full lg:w-2/4 overflow-y-auto relative">

        <!-- Dialog content goes here -->

        <div class="text-3xl w-full mb-4 flex items-center ">
          <div class="w-full">{{ selectedPost.title }}</div>
          <button class="flex flex-col justify-items-end items-end mr-2" @click="toggleFullScreen">
            <svg v-if="!isFullScreen" class="w-9 w-9" viewBox="0 0 19.5 19.5" xmlns="http://www.w3.org/2000/svg">
              <path d="M5.53,5.21a.32.32,0,0,0-.32.32v2.6a.33.33,0,0,1-.65,0V5.53a1,1,0,0,1,1-1h2.6a.33.33,0,0,1,0,.65Z"
                    style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M11.05,4.88a.32.32,0,0,1,.32-.32H14a1,1,0,0,1,1,1v2.6a.33.33,0,0,1-.65,0V5.53A.32.32,0,0,0,14,5.21h-2.6a.32.32,0,0,1-.32-.33"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M4.88,11.05a.32.32,0,0,1,.33.32V14a.32.32,0,0,0,.32.32h2.6a.33.33,0,0,1,0,.65H5.53a1,1,0,0,1-1-1h0v-2.6a.32.32,0,0,1,.32-.32"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M14.62,11.05a.32.32,0,0,1,.32.32V14a1,1,0,0,1-1,1h-2.6a.33.33,0,0,1,0-.65H14a.32.32,0,0,0,.32-.32v-2.6a.32.32,0,0,1,.33-.32"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <circle cx="9.75" cy="9.75" r="9"
                      style="fill:none;stroke:#fff;stroke-linecap:round;stroke-linejoin:round;stroke-width:1.5px"/>
            </svg>
            <svg v-if="isFullScreen" class="w-9 w-9" viewBox="0 0 19.5 19.5" xmlns="http://www.w3.org/2000/svg">
              <circle cx="9.75" cy="9.75" r="9"
                      style="fill:none;stroke:#fff;stroke-linecap:round;stroke-linejoin:round;stroke-width:1.5px"/>
              <path
                  d="M7.91,3.87a.37.37,0,0,1,.37.37V7.18a1.1,1.1,0,0,1-1.1,1.1H4.24a.37.37,0,1,1,0-.73H7.18a.38.38,0,0,0,.37-.37V4.24a.37.37,0,0,1,.36-.37"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M11.59,3.87a.37.37,0,0,1,.36.37V7.18a.38.38,0,0,0,.37.37h2.94a.37.37,0,1,1,0,.73H12.32a1.1,1.1,0,0,1-1.1-1.1V4.24a.37.37,0,0,1,.37-.37"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M3.87,11.59a.37.37,0,0,1,.37-.37H7.18a1.1,1.1,0,0,1,1.1,1.1v2.94a.37.37,0,1,1-.73,0V12.32A.38.38,0,0,0,7.18,12H4.24a.37.37,0,0,1-.37-.36"
                  style="stroke:#fff;stroke-miterlimit:10"/>
              <path
                  d="M11.22,12.32a1.1,1.1,0,0,1,1.1-1.1h2.94a.37.37,0,1,1,0,.73H12.32a.38.38,0,0,0-.37.37v2.94a.37.37,0,1,1-.73,0Z"
                  style="stroke:#fff;stroke-miterlimit:10"/>
            </svg>
          </button>
          <button class="flex flex-col justify-items-end items-end" @click="closeCustomWindow">

            <svg class="w-11 h-11" fill="none" stroke="currentColor" stroke-width="1.5"
                 viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path d="m9.75 9.75 4.5 4.5m0-4.5-4.5 4.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" stroke-linecap="round"
                    stroke-linejoin="round"/>
            </svg>
          </button>

        </div>
        <hr>
        <div v-if="isFullScreen" class="mt-4" v-html="selectedPost.content">
        </div>
        <div v-if="!isFullScreen" >
          <h1 class="font-bold mt-4">Short description:</h1>
          <div class="mt-4" v-html="selectedPost.excerpt">

          </div>
        </div>
        <div v-if="!isFullScreen" class="mt-4">
          <button class="flex flex-col justify-items-end items-end mr-2 hover:underline" @click="toggleFullScreen">Learn more</button>
        </div>
        <!-- Button to close the dialog -->

      </div>
    </div>
  </div>

  <!-- S
  <div>
    <div class="flex justify-center border-b-2 rounded-xl items-center w-full p-5">
    <h1 class="text-3xl font-black">Blog</h1>
    </div>
  <div  class="grid grid-cols-1 xl:grid-cols-3 gap-y-10 gap-x-10 items-start p-8">
    <div v-for="post in posts" :key="post.id" class="max-w-2xl ml-10 my-8 p-6 bg-white rounded-md">
        <h3 class="text-2xl font-bold mb-4 hover:underline">{{ post.title }} </h3>
  </div>
  </div>
  </div>
  -->
</template>
<script>
import VideoBackground from 'vue-responsive-video-background-player';
import axios from 'axios';
import 'vuejs-dialog/dist/vuejs-dialog.min.css';
import IconInfo from "@/components/icons/IconInfo.vue";
import IconLocation from "@/components/icons/IconLocation.vue";
import IconUser from "@/components/icons/IconUser.vue";
import IconStack from "@/components/icons/IconStack.vue";
import IconLink from "@/components/icons/IconLink.vue";
import IconReact from "@/components/icons/IconReact.vue";
import IconDate from "@/components/icons/IconDate.vue";
import MascotIcon from "@/components/icons/MascotIcon.vue";

export default {
  components: {VideoBackground, IconInfo, IconLocation, IconUser, IconStack, IconLink, IconReact, IconDate, MascotIcon},
  data() {
    return {
      githubUsername: 'h1ushkov',
      userData: {},
      posts: [],
      projects: [],
      customWindowVisible: false,
      selectedProject: null,
      selectedPost: null,
      isFullScreen: false,
    };
  },
  mounted() {
    this.fetchPosts();
    // Fetching projects should be done after fetching posts, not parallel to avoid the error
    this.fetchProjects();
    axios.get(`https://api.github.com/users/${this.githubUsername}`)
        .then(response => {
          this.userData = {
            avatarUrl: response.data.avatar_url,
            bio: response.data.bio,
            location: response.data.location,
            name: response.data.name,
            login: response.data.login,
            public_repos: response.data.public_repos
          };
        })
        .catch(error => {
          console.error('User credentials error:', error);
        });
  },
  methods: {
    selectProject(project) {
      this.selectedProject = project;
    },
    selectPost(post) {
      this.selectedPost = post;
    },
    closeCustomWindow() {
      this.selectedProject = null;
      this.selectedPost = null;
      console.log('Custom window closed');
    },
    toggleFullScreen() {
      this.isFullScreen = !this.isFullScreen;
    },
    async fetchPosts() {
      try {
        const response = await axios.get('https://public-api.wordpress.com/rest/v1.1/sites/hlushkov.wordpress.com/posts/?category=blog');
        this.posts = response.data.posts.map(post => ({
          title: post.title,
          excerpt: post.excerpt,
          date: post.date,
          content: post.content,
          featuredImage: post.featured_image || null,
        }));
      } catch (error) {
        console.error('Error fetching WordPress posts:', error);
      }
    },
    formatTime(dateString) {
      const options = {year: 'numeric', month: 'long', day: 'numeric'};
      return new Date(dateString).toLocaleDateString(undefined, options);
    },
    async fetchProjects() {
      try {
        const response = await axios.get('https://public-api.wordpress.com/rest/v1.1/sites/hlushkov.wordpress.com/posts/?category=project');
        this.projects = response.data.posts.map(project => ({
          title: project.title,
          excerpt: project.excerpt,
          date: project.date,
          content: project.content,
          featuredImage: project.featured_image || null,
        }));
      } catch (error) {
        console.error('Error fetching WordPress projects:', error);
      }
    },
  }
};
</script>

<style scoped>
.fullscreen {
  @apply w-screen rounded-none border-none text-[22px] font-normal text-pretty lg:pl-56 lg:pr-56 h-screen transition-all font-serif;
}
.fullscreenbg{
  @apply dark:bg-neutral-950 bg-white;
}
::-webkit-scrollbar {
  @apply w-1;
}

/* Track */
::-webkit-scrollbar-track {
  background: transparent;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: transparent;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: white;
}
</style>
