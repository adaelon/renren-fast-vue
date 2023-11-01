<template>
    <div class="course-container">
      <div class="course-banner">
        <h1>{{ course.courseName }}</h1>
      </div>
      <button @click="joinCourse" class="join-button">立即参与</button>
  
      <div class="course-reviews">
        <h2>课程评价</h2>
        <p v-if="reviews.length === 0">暂无评价</p>
        <div v-for="review in reviews" :key="review.id">
          <p>{{ review.content }}</p>
        </div>
      </div>
  
      <div class="course-teachers">
        <h2>授课教师</h2>
        <div v-for="teacher in teachers" :key="teacher.id" class="teacher-card">
          <img :src="teacher.avatar" alt="teacher avatar">
          <p>{{ teacher.name }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
import axios from 'axios'

export default {
  data () {
    return {
      course: {},
      reviews: [],
      teachers: []
    }
  },
  created () {
    this.fetchCourseData(1)
    this.fetchReviews()
    this.fetchTeachers()
  },
  methods: {
    async fetchCourseData (courseId) {
      const response = await axios.get(`${window.SITE_CONFIG.baseUrl}/course/courses/info/${courseId}`)
      this.course = response.data.courses
      console.log(response.data)
    },
    async fetchReviews () {
      const response = await axios.get('/api/reviews')
      this.reviews = response.data
    },
    async fetchTeachers () {
      const response = await axios.get('/api/teachers')
      this.teachers = response.data
    },
    async joinCourse () {
      await axios.post('/api/joinCourse', { courseId: this.course.id })
      alert('参与成功！')
    }
  }

}
</script>
  
  <style scoped>
  .course-container {
    width: 80%;
    margin: 20px auto;
    padding: 20px;
    background-color: black;
    border-radius: 10px;
  }
  
  .course-banner {
    background: url('https://img-home.csdnimg.cn/images/20230817060240.png') center/cover no-repeat;
    padding: 40px;
    color: white;
    text-align: center;
  }
  
  .join-button {
    display: block;
    margin: 20px auto;
    padding: 10px 20px;
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
  }
  
  .course-reviews, .course-teachers {
    padding: 20px;
    background-color: white;
    margin-top: 10px;
    border-radius: 10px;
  }
  
  .teacher-card {
    display: flex;
    align-items: center;
    padding: 10px;
  }
  
  .teacher-card img {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    margin-right: 10px;
  }
  </style>
  