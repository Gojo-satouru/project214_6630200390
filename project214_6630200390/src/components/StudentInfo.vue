<template>
  <div class="card p-3 shadow-sm border-success text-center">
    <h2 class="text-success"><i class="fas fa-user-graduate"></i> ข้อมูลนิสิต</h2>

    <div v-if="student.studentID">
      <div class="image-container">
        <img :src="profileImages[currentImageIndex]" alt="Profile" class="rounded-circle border border-success" width="150">
        <div class="image-buttons">
          <button class="btn btn-secondary" @click="prevImage">←</button>
          <button class="btn btn-secondary" @click="nextImage">→</button>
        </div>
      </div>

      <p><strong>รหัสนิสิต:</strong> {{ student.studentID }}</p>
      <p><strong>ชื่อ-นามสกุล:</strong> {{ student.name }}</p>
      <p><strong>สาขา:</strong> {{ student.major }}</p>
      <p><strong>โรงเรียนเดิม:</strong> {{ student.school }}</p>
      <button class="btn btn-warning" @click="editMode = true">แก้ไขข้อมูล</button>
    </div>

    <p v-else class="text-danger">❌ ไม่พบข้อมูลนิสิต</p>

    <div v-if="editMode" class="mt-3">
      <input v-model="student.name" class="form-control mb-2">
      <input v-model="student.major" class="form-control mb-2">
      <input v-model="student.school" class="form-control mb-2">
      <button class="btn btn-success" @click="updateStudent">บันทึก</button>
      <button class="btn btn-danger" @click="editMode = false">ยกเลิก</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      student: {},
      editMode: false,
      profileImages: [
        '/punnawich.jpg',
        '/punnawich2.jpg',
        '/punnawich3.jpg',
      ],
      currentImageIndex: 0,
    };
  },
  mounted() {
    this.fetchStudent();
  },
  methods: {
    fetchStudent() {
      console.log("📡 Fetching student data...");
      fetch("http://localhost:3000/students/1")
        .then(res => {
          if (!res.ok) {
            throw new Error("❌ Failed to fetch student data.");
          }
          return res.json();
        })
        .then(data => {
          this.student = data;
          console.log("✅ Student data loaded:", this.student);
        })
        .catch(error => {
          console.error("❌ Error fetching student data:", error);
        });
    },

    nextImage() {
      this.currentImageIndex = (this.currentImageIndex + 1) % this.profileImages.length;
    },

    prevImage() {
      this.currentImageIndex = (this.currentImageIndex - 1 + this.profileImages.length) % this.profileImages.length;
    },

    updateStudent() {
      if (!this.student.studentID) {
        console.error("❌ Error: Student ID is missing");
        return;
      }

      fetch(`http://localhost:3000/students/${this.student.id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.student),
      })
        .then(res => {
          if (!res.ok) {
            throw new Error("❌ Failed to update student data.");
          }
          return res.json();
        })
        .then(() => {
          console.log("✅ Updated student successfully");
          this.editMode = false;
        })
        .catch(error => {
          console.error("❌ Error updating student:", error);
        });
    }
  }
};
</script>

<style scoped>
.image-container {
  position: relative;
}

.image-buttons {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 10px;
}

.image-buttons button {
  font-size: 18px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

img {
  transition: transform 0.3s ease;
}

button {
  transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
  background-color: #28a745;
  transform: scale(1.1);
}
</style>
