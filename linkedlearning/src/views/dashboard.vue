<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
    <FindBar :getsearchs="getshowsearch" @click="getsearchcourse"/>
    <AdvertismentBar />
    <div>
      <h2 class="cur">Search by catergories</h2>
      <span  v-for="category in categories" :key="category._id" @click="selectCategory(category._id)">
        <b  class="select-category">{{ category.title }}</b>
      </span>
    </div>
    <hr />
    <div>
      <h2 class="cur">Currently learning</h2>
      <div class="row" id="cards">
        <CourseCard v-for="course in limitedenroll" :key="course._id" :courses="course" />

      </div>
      <div id="viewdiv">
        <div id="viewall" @click="viewallpage"><b>View all</b></div>
      </div>
      <AdvertBar />
      <h2 class="cur">Recommended Courses</h2>
      <div class="row" id="cards">
        
        <CourseCard
          v-for="course in courses"
          :key="course._id"
          :courses="course"
          @click="getshowcourse(course._id)"
        >
        </CourseCard>
      </div>
    </div>
  </div>
</template>

<script>
import FindBar from "../components/FindBar.vue";
import AdvertismentBar from "../components/AdvertismentBar.vue";
import AdvertBar from "../components/AdvertBar.vue";
import CourseCard from "../components/CourseCard.vue";
import axios from "axios";
import router from "../router/index";

export default {
  name: "App",
  async created() {
    this.getcourse(), this.getcategory();
    const response = await axios.get('/course/enrolled');
    console.log(response.data);
    this.enrolledCourses = response.data.enrolledCourses
    if(this.enrolledCourses.length<3){
      for(var i=0;i<this.enrolledCourses.length;i++){
      this.limitedenroll[i]=this.enrolledCourses[i]
    }
    console.log(this.limitedenroll)

    }
    else{
      console.log('ho')
      for(i=0;i<=2;i++){
        
      this.limitedenroll[i]=this.enrolledCourses[i]
    }
    }
      
    
  },
  data() {
    return {
      courses: [],
      categories: [],
      enrolledCourses:[],
      limitedenroll:[],
      errorMsg: "",
      title:"",
    };
  },
  methods: {
    async getcourse() {
      try {
        const response = await axios.get("course/");
        console.log(response.data);
        this.courses = response.data.courses;
      } catch (error) {
        this.errorMsg = "Error retreving data";
        console.log(error);
      }
    },
    async getcategory() {
      try {
        const response = await axios.get("course/category");
        console.log(response.data);
        this.categories = response.data.categories;
      } catch (error) {
        this.errorMsg = "Error retreving data";
        console.log(error);
      }
    },
    async getsearchcourse(){
      try {
        const response = await axios.get(`course/search?title=${this.title}`);
        console.log(response.data);
        this.courses = response.data.courses;
      } catch (error) {
        this.errorMsg = "Error retreving data";
        console.log(error);
      }
    },
    viewallpage() {
      router.push({ name: "viewEnroll" });
    },
    getshowcourse(course_id) {
      console.log("HIt");
      router.push({ path: `/course/${course_id}/details` });
    },
    getshowsearch(title){
      console.log(title)
      this.title = title
    },
    async selectCategory(category_id){
      try {
        console.log('sdf')
        const response  = await axios.get(`/course/bycat?categoryId=${category_id}`)
        console.log(response.data);
        console.log(category_id);
        console.log('a;lsds;a')
        this.courses = response.data.courses;
      } catch (error) {
        this.errorMsg = "Error retreving data";
        console.log(error);
      }
      
    }
  },
  components: {
    FindBar,
    AdvertismentBar,
    AdvertBar,
    CourseCard,
  },
}
</script>

<style>
.select-category {
  margin: 2vw 2vw;
}
.cur {
  margin: 2vw 2vw;
}
#cards {
  justify-content: left;
  align-items: center;
}
#viewall {
  justify-content: center;
  text-align: center;
  width: 8vw;
  height: 2vw;
  color: blueviolet;
}
#viewdiv {
  width: 80vw;
  display: flex;
  justify-content: center;
  align-items: center;
}

@import "~bootstrap/dist/css/bootstrap.css";
</style>
