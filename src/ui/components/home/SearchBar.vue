
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
              @input="filterSchools"
              placeholder="Search School Here..."
              class="search-input"
            />
            
          </div>
        </div>

        <!-- Table Section -->
        <div>
          <table
            style="border: 1px solid gray; width: 100%; text-align: left; border-collapse: collapse;"
          >
            <thead>
              <tr>
                <th style="border: 1px solid gray; padding: 8px;">SN</th>
                <th style="border: 1px solid gray; padding: 8px;">School Logo</th>
                <th style="border: 1px solid gray; padding: 8px;">School Name</th>
                <th style="border: 1px solid gray; padding: 8px;">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(school, index) in filteredSchools" :key="school.id">
                <td style="border: 1px solid gray; padding: 8px;">
                  {{ index + 1 }}
                </td>
                <td style="border: 1px solid gray; padding: 8px;">
                  <img
                    :src="school.logo || fallbackLogo"
                    alt="School Logo"
                    class="school-logo"
                  />
                </td>
                <td style="border: 1px solid gray; padding: 8px;">
                  {{ school.school_name }}
                </td>
                <td style="border: 1px solid gray; padding: 8px;">
                  <button @click="selectSchool(school)" class="action-button">
                    Join Campaign
                  </button>
                </td>
              </tr>
              <tr v-if="filteredSchools.length<=0">
                <td colspan="4" style="text-align: center; padding: 8px;">
                  No schools found.
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <div v-if="isLoading" class="loading">Loading schools...</div>
        <div v-if="error" class="error">{{ error }}</div>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import debounce from "lodash/debounce";

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
    async fetchSchools(search) {
      this.isLoading = true;
      this.error = null;
      let URL =
        "https://api.devharlemwizardsinabox.com/campaign/campaign_school_list/";
      if (search) {
        URL = `${URL}?search=${search}`;
      }
      try {
        const response = await axios.get(URL);
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
    debounce(func, delay) {
      let timer;
      return function (...args) {
        clearTimeout(timer);
        timer = setTimeout(() => func.apply(this, args), delay);
      };
    },
    filterSchools() {
      const debouncedFetch = this.debounce(() => {
        const query = this.searchQuery.toLowerCase();
        this.fetchSchools(query);
      }, 300); // Adjust the delay as needed
      debouncedFetch();
    },
    selectSchool(school) {
      alert(`Selected School: ${school.school_name}`);
      this.searchQuery = school.school_name;
      this.showDropdown = false;
    },
  
    handleImageError(event) {
      event.target.src = this.fallbackLogo;
    },
  },
  mounted() {
    this.fetchSchools("");
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

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  border: 1px solid gray;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f5f5f5;
}

.school-logo {
  width: 50px;
  height: 50px;
  object-fit: cover;
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
  /* height: 300px; */
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
  margin-bottom: 30px;
  padding: 30px;
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
