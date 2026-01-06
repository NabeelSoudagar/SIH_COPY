<template>
  <nav class="navbar">
    <ul>
      <li><RouterLink to="/">{{ $t("Home") }}</RouterLink></li>
      <li><RouterLink to="/signup">{{ $t("Signup") }}</RouterLink></li>
      <li><RouterLink to="/login">{{ $t("Login") }}</RouterLink></li>
      <li v-if="isAuthenticated"><RouterLink to="/video-call">{{ $t("LiveCall") }}</RouterLink></li>
      <li v-if="isAuthenticated && userRole === 'patient'"><RouterLink to="/patient-dashboard">{{ $t("PatientDashboard") }}</RouterLink></li>
      <li v-if="isAuthenticated && userRole === 'doctor'"><RouterLink to="/doctor-dashboard">{{ $t("DoctorDashboard") }}</RouterLink></li>
      <li><RouterLink to="/medical-store">{{ $t("MedicalStore") }}</RouterLink></li>
      <li v-if="isAuthenticated && userRole === 'patient'"><RouterLink to="/book-consultation">{{ $t("BookConsultation") }}</RouterLink></li>


      <!-- Language Buttons -->
      <li class="lang-buttons">
        <button @click="setLanguage('en')">EN</button>
        <button @click="setLanguage('pa')">ਪੰਜਾਬੀ</button>
        <button @click="setLanguage('hi')">हिन्दी</button>
      </li>
    </ul>
  </nav>
</template>

<script setup>
import { RouterLink } from "vue-router"
import { useI18n } from "vue-i18n"
import { useAuthStore } from "../store/auth"
import { computed, watchEffect } from "vue"

const { locale } = useI18n()
const authStore = useAuthStore()

const isAuthenticated = computed(() => {
  const hasToken = !!authStore.token
  console.log('Auth check - hasToken:', hasToken, 'token:', authStore.token)
  return hasToken
})

const userRole = computed(() => {
  const role = authStore.user?.role
  console.log('Role check - user:', authStore.user, 'role:', role)
  return role
})

// Watch for auth changes
watchEffect(() => {
  console.log('Auth state changed:', { 
    isAuthenticated: isAuthenticated.value, 
    userRole: userRole.value,
    user: authStore.user,
    token: authStore.token
  })
})

function setLanguage(lang) {
  locale.value = lang
}
</script>

<style scoped>
.navbar {
  background: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
  padding: 1rem 2rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  position: sticky;
  top: 0;
  z-index: 1000;
  transition: all 0.3s ease;
}

.navbar:hover {
  box-shadow: 0 6px 30px rgba(0, 0, 0, 0.15);
}

ul {
  display: flex;
  list-style: none;
  gap: 0.5rem;
  align-items: center;
  margin: 0;
  padding: 0;
}

li {
  position: relative;
  overflow: hidden;
}

a {
  color: #ecf0f1;
  text-decoration: none;
  padding: 0.75rem 1.25rem;
  border-radius: 8px;
  font-weight: 500;
  letter-spacing: 0.5px;
  position: relative;
  display: block;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  overflow: hidden;
}

a::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s ease;
}

a:hover::before {
  left: 100%;
}

a:hover {
  color: #ffffff;
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

a.router-link-active {
  background: linear-gradient(135deg, #3498db, #2980b9);
  color: white;
  font-weight: 600;
  box-shadow: 0 4px 15px rgba(52, 152, 219, 0.3);
  transform: translateY(-1px);
}

a.router-link-active:hover {
  background: linear-gradient(135deg, #2980b9, #3498db);
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(52, 152, 219, 0.4);
}

/* Language buttons */
.lang-buttons {
  display: flex;
  gap: 0.5rem;
  margin-left: auto;
}

.lang-buttons button {
  padding: 0.5rem 1rem;
  border: 2px solid rgba(255, 255, 255, 0.2);
  border-radius: 25px;
  background: rgba(255, 255, 255, 0.1);
  color: #ecf0f1;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.85rem;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.lang-buttons button::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: width 0.3s ease, height 0.3s ease;
}

.lang-buttons button:hover::before {
  width: 200px;
  height: 200px;
}

.lang-buttons button:hover {
  background: linear-gradient(135deg, #1abc9c, #16a085);
  color: white;
  border-color: #1abc9c;
  transform: translateY(-2px) scale(1.05);
  box-shadow: 0 4px 15px rgba(26, 188, 156, 0.3);
}

.lang-buttons button:active {
  transform: translateY(0) scale(0.98);
}

/* Responsive design */
@media (max-width: 768px) {
  .navbar {
    padding: 0.75rem 1rem;
  }
  
  ul {
    flex-wrap: wrap;
    gap: 0.25rem;
  }
  
  a {
    padding: 0.5rem 0.75rem;
    font-size: 0.9rem;
  }
  
  .lang-buttons {
    margin-left: 0;
    margin-top: 0.5rem;
  }
  
  .lang-buttons button {
    padding: 0.4rem 0.8rem;
    font-size: 0.8rem;
  }
}

/* Animation keyframes */
@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.navbar {
  animation: fadeInDown 0.6s ease-out;
}
</style>
