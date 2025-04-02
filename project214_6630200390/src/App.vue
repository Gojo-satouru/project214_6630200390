<template>
  <div>
    <div class="logo-container">
      <img src="/logo.jpg" alt="University Logo" class="logo">
    </div>
    <div class="container">
      <h1 class="mt-3 text-center text-success">
        <i class="fas fa-laptop-code"></i> ประวัติ
      </h1>
      <StudentInfo />
      <CourseList />
    </div>
  </div>
</template>

<script>
import StudentInfo from './components/StudentInfo.vue'
import CourseList from './components/CourseList.vue'

export default {
  components: { StudentInfo, CourseList }
}
</script>

<style>

body {
  background-color: #808080; 
  background-image: url('/newback.gif'); 
  background-size: cover; 
  background-position: center center; 
  background-repeat: no-repeat; 
  font-family: 'Courier New', Courier, monospace;
  color: #333;
  margin: 0;
  padding: 0;
  height: 100vh; 
}


.logo-container {
  position: absolute;
  top: 15px;
  left: 15px;
}

.logo {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border: 3px solid #007bff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

h1 {
  font-size: 2.5rem;
  color: #007bff;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  padding: 10px 20px; 
  background-color: white; 
  border-radius: 10px; 
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1); 
  display: inline-block; 
}

.fas {
  margin-right: 8px;
}

.container {
  margin-top: 120px;
}

h1 {
  font-size: 2.5rem;
  color: #007bff;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}
</style>
