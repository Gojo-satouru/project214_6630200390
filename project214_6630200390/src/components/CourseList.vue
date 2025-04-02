<template>
  <div class="card p-3 shadow-sm border-primary">
    <h3 class="text-primary">📚 รายวิชา</h3>

    <button class="btn btn-primary mb-3" @click="addNewCourse">➕ เพิ่มรายวิชา</button>

    <table class="table">
      <thead>
        <tr>
          <th>รหัสวิชา</th>
          <th>ชื่อวิชา</th>
          <th>หน่วยกิต</th>
          <th>เกรด</th>
          <th>จัดการ</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="course in courses" :key="course.id">
          <td>{{ course.code }}</td>
          <td>{{ course.name }}</td>
          <td>{{ course.credit }}</td>
          <td>{{ course.grade }}</td>
          <td>
            <button class="btn btn-warning btn-sm" @click="editCourse(course)">✏️</button>
            <button class="btn btn-danger btn-sm" @click="deleteCourse(course.id)">🗑</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="isEditing">
      <h4>{{ newCourse.id ? '✏️ แก้ไขรายวิชา' : '➕ เพิ่มรายวิชา' }}</h4>
      <input v-model="newCourse.code" placeholder="รหัสวิชา" class="form-control mb-2">
      <input v-model="newCourse.name" placeholder="ชื่อวิชา" class="form-control mb-2">
      <input v-model="newCourse.credit" placeholder="หน่วยกิต" type="number" class="form-control mb-2">
      <input v-model="newCourse.grade" placeholder="เกรด" class="form-control mb-2">
      <button class="btn btn-success" @click="saveCourse">บันทึก</button>
      <button class="btn btn-secondary" @click="cancelEdit">ยกเลิก</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return { 
      courses: [], 
      newCourse: { id: null, code: "", name: "", credit: "", grade: "" },
      isEditing: false 
    };
  },
  mounted() {
    this.fetchCourses();
  },
  methods: {
    fetchCourses() {
      fetch("http://localhost:3000/courses")
        .then(res => {
          if (!res.ok) throw new Error("Failed to fetch courses data.");
          return res.json();
        })
        .then(data => {
          this.courses = data;
        })
        .catch(error => {
          console.error("Error fetching data:", error);
        });
    },

    saveCourse() {
      if (this.newCourse.id) {
        fetch(`http://localhost:3000/courses/${this.newCourse.id}`, {
          method: "PUT",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(this.newCourse),
        })
        .then(res => {
          if (!res.ok) throw new Error("Failed to update course data.");
          return res.json();
        })
        .then(() => {
          this.fetchCourses();
          this.cancelEdit();
        })
        .catch(error => console.error("Error updating course data:", error));
      } else {
      
        fetch("http://localhost:3000/courses", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ 
            code: this.newCourse.code,
            name: this.newCourse.name,
            credit: this.newCourse.credit,
            grade: this.newCourse.grade
          }),
        })
        .then(res => {
          if (!res.ok) throw new Error("Failed to add new course.");
          return res.json();
        })
        .then(() => {
          this.fetchCourses();
          this.cancelEdit();
        })
        .catch(error => console.error("Error saving course data:", error));
      }
    },

    deleteCourse(id) {
      fetch(`http://localhost:3000/courses/${id}`, { method: "DELETE" })
        .then(res => {
          if (!res.ok) throw new Error(`Failed to delete course data. Response status: ${res.status}`);
          return res.text();
        })
        .then(() => {
          this.fetchCourses();
        })
        .catch(error => console.error("Error deleting course data:", error));
    },

    editCourse(course) {
      this.newCourse = { ...course };
      this.isEditing = true; 
    },

    addNewCourse() {
      this.newCourse = { id: null, code: "", name: "", credit: "", grade: "" };
      this.isEditing = true; 
    },

    cancelEdit() {
      this.newCourse = { id: null, code: "", name: "", credit: "", grade: "" };
      this.isEditing = false; 
    }
  }
};
</script>
