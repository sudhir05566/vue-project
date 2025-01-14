<template>
  <div class="container">
    <div class="content-box">
      <section class="call-to-action">
        <!-- Main Heading -->
        <h2>Are you ready to take the challenge?</h2>
        <p>Download Hariem Wizards App</p>

        <!-- App Download Section -->
        <div class="app-download">
          <img src="../../../../public/goolge-store.png" alt="Google Store" />
          <img src="../../../../public/apple-store.png" alt="Apple Store" />
        </div>

        <!-- Divider Section -->
        <div class="divider-text">
          <span class="line"></span>
          <p>or you can signup right now</p>
          <span class="line"></span>
        </div>

        <!-- Search Bar -->
        <div class="searchbar">
          <div class="search-bar-dropdown">
            <input
              type="text"
              v-model="searchQuery"
              @focus="showDropdown = true"
              @blur="hideDropdown"
              @input="filterSchools"
              placeholder="Search School Here..."
              class="search-input"
            />
            <div
              v-if="showDropdown && filteredSchools.length > 0"
              class="dropdown"
            >
              <ul>
                <li
                  v-for="school in filteredSchools"
                  :key="school.id"
                  class="dropdown-item"
                >
                  <div class="dropdown-content">
                    <img
                      :src="school.logo || fallbackLogo"
                      alt="School Logo"
                      class="school-logo"
                      @click="selectSchool(school)"
                    />
                    <span @click="selectSchool(school)">
                      {{ school.school_name }}
                    </span>
                    <button
                      @click.stop="selectSchool(school)"
                      class="action-button"
                    >
                      Join Campaign
                    </button>
                  </div>
                </li>
              </ul>
            </div>
            <div
              v-else-if="showDropdown && !filteredSchools.length"
              class="dropdown"
            >
              <p>No schools found.</p>
            </div>
          </div>
        </div>

      
        <div v-if="isLoading" class="loading">Loading schools...</div>
        <div v-if="error" class="error">{{ error }}</div>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      searchQuery: "",
      showDropdown: false,
      schools: [],
      filteredSchools: [],
      isLoading: false,
      error: null,
      fallbackLogo: "../../../../public/fallbacklogo.jpg",
    };
  },
  methods: {
    async fetchSchools() {
      this.isLoading = true;
      this.error = null;
      try {
        const response = await axios.get(
          "https://api.devharlemwizardsinabox.com/campaign/campaign_school_list/"
        );
        if (response.data && response.data.school_list) {
          this.schools = response.data.school_list;
          this.filteredSchools = this.schools;
        } else {
          this.error = "No schools found.";
        }
      } catch (err) {
        this.error = "Failed to fetch schools. Please try again later.";
        console.error(err);
      } finally {
        this.isLoading = false;
      }
    },
    filterSchools() {
      const query = this.searchQuery.toLowerCase();
      this.filteredSchools = this.schools.filter((school) =>
        school.school_name.toLowerCase().includes(query)
      );
    },
    selectSchool(school) {
      alert(`Selected School: ${school.school_name}`);
      this.searchQuery = school.school_name;
      this.showDropdown = false;
    },
    hideDropdown() {
      setTimeout(() => {
        this.showDropdown = false;
      }, 200);
    },
    handleImageError(event) {
      event.target.src = this.fallbackLogo;
    },
  },
  mounted() {
    this.fetchSchools();
  },
};
</script>


<style scoped>
.loading {
  text-align: center;
  color: #555;
  margin-top: 10px;
}

.error {
  text-align: center;
  color: red;
  margin-top: 10px;
}

/* Main container */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 100px;
}

/* Content box */
.content-box {
  text-align: center;
  background: white;
  border-radius: 8px;
  border: 1px solid #e0e0e0;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 10px;
  max-width: 600px;
  height: 300px;
  width: 100%;
}

.call-to-action {
  text-align: center;
  padding: 10px;
}
.app-download img {
  width: 150px;
  margin: 10px;
}
.campaigns {
  margin-top: 20px;
}
.join-btn {
  background-color: #ff004d;
  color: white;
  padding: 10px 15px;
  margin: 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

h2 {
  color: hsl(346.89deg 64.26% 46.08%);
  font-size: 20px;
  font-weight: bold;
}

/* Divider with text */
.divider-text {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 20px 0;
}

.divider-text p {
  margin: 0 10px;
  white-space: nowrap;
}

.divider-text .line {
  flex: 1;
  height: 1px;
  max-width: 100px;
  background-color: #e0e0e0;
}

.searchbar {
  display: flex;
  justify-content: center;
}

.search-bar-dropdown {
  position: absolute;
  width: 400px;
  align-items: center;
  justify-content: center;
}
.search-input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  background: white;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-top: 4px;
  z-index: 1000;
}
.dropdown ul {
  list-style: none;
  padding: 0;
  margin: 0;
  width: 100%;
}
.dropdown-item {
  padding: 8px;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.dropdown-item:last-child {
  border-bottom: none;
}
.dropdown-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: -webkit-fill-available;
}
.nested-dropdown {
  margin-top: 4px;
  margin-left: 16px;
}
.action-button {
  background-color: white;
  color: hsl(346.89deg 64.26% 46.08%);
  border: none;
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
  border: hsl(346.89deg 64.26% 46.08%);
  border: 1px solid hsl(346.89deg 64.26% 46.08%);
}
.action-button:hover {
  background-color: white;
}

.school-logo{
  width: 50px;
  height: 50px;
}
</style>
