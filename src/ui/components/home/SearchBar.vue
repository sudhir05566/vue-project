<template>
  <div class="container">
    <div class="content-box">
      <section class="call-to-action">
        <!-- Main Heading -->
        <h2 class="introheading">Are you ready to take the challenge?</h2>
        <p class="intropara">Download Hariem Wizards App</p>

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
            style="
              border: 1px solid gray;
              width: 100%;
              text-align: left;
              border-collapse: collapse;
            "
          >
            <thead>
              <tr>
                <th style="border: 1px solid gray; padding: 8px">SN</th>
                <th style="border: 1px solid gray; padding: 8px">
                  School Logo
                </th>
                <th style="border: 1px solid gray; padding: 8px">
                  School Name
                </th>
                <th style="border: 1px solid gray; padding: 8px">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(school, index) in filteredSchools" :key="school.id">
                <td style="border: 1px solid gray; padding: 8px">
                  {{ index + 1 }}
                </td>
                <td style="border: 1px solid gray; padding: 8px">
                  <img
                    :src="school.logo || fallbackLogo"
                    alt="School Logo"
                    class="school-logo"
                  />
                </td>
                <td style="border: 1px solid gray; padding: 8px">
                  {{ school.school_name }}
                </td>
                <td style="border: 1px solid gray; padding: 8px">
                  <button @click="selectSchool(school)" class="action-button">
                    Join Campaign
                  </button>
                </td>
              </tr>
              <tr v-if="filteredSchools.length <= 0">
                <td colspan="4" style="text-align: center; padding: 8px">
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
import axios from 'axios'
import debounce from 'lodash/debounce'

export default {
  data() {
    return {
      searchQuery: '',
      showDropdown: false,
      schools: [],
      filteredSchools: [],
      isLoading: false,
      error: null,
      fallbackLogo: '../../../../public/fallbacklogo.jpg',
    }
  },
  methods: {
    async fetchSchools(search) {
      this.isLoading = true
      this.error = null
      let URL =
        'https://api.devharlemwizardsinabox.com/campaign/campaign_school_list/'
      if (search) {
        URL = `${URL}?search=${search}`
      }
      try {
        const response = await axios.get(URL)
        if (response.data && response.data.school_list) {
          this.schools = response.data.school_list
          this.filteredSchools = this.schools
        } else {
          this.error = 'No schools found.'
        }
      } catch (err) {
        this.error = 'Failed to fetch schools. Please try again later.'
        console.error(err)
      } finally {
        this.isLoading = false
      }
    },
    debounce(func, delay) {
      let timer
      return function (...args) {
        clearTimeout(timer)
        timer = setTimeout(() => func.apply(this, args), delay)
      }
    },
    filterSchools() {
      const debouncedFetch = this.debounce(() => {
        const query = this.searchQuery.toLowerCase()
        this.fetchSchools(query)
      }, 300)
      debouncedFetch()
    },
    selectSchool(school) {
      alert(`Selected School: ${school.school_name}`)
      this.searchQuery = school.school_name
      this.showDropdown = false
    },

    handleImageError(event) {
      event.target.src = this.fallbackLogo
    },
  },
  mounted() {
    this.fetchSchools('')
  },
}
</script>

<style scoped>
/* Main container */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 100px;
  padding: 10px;
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
  width: 100%;
}

/* Call-to-action section */
.call-to-action {
  text-align: center;
  padding: 10px;
}

.app-download img {
  width: 120px;
  margin: 10px;
}

.introheading, intropara{
          color: hsl(346.89deg 64.26% 46.08%);
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

/* Search bar */
.searchbar {
  /* display: flex; */
  justify-content: center;
  margin-bottom: 30px;
  padding: 30px;
}

.search-input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}

/* Table */
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
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

/* Action button */
.action-button {
  background-color: white;
  color: hsl(346.89deg 64.26% 46.08%)!important;
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

/* Responsive Styles */
@media (max-width: 768px) {
  .content-box {
    padding: 15px;
  }

  .app-download img {
    width: 100px;
  }

  th,
  td {
    font-size: 14px;
    padding: 6px;
  }

  .search-input {
    font-size: 14px;
    padding: 6px;
  }

  .divider-text {
    flex-direction: column;
    gap: 10px;
  }

  .divider-text .line {
    max-width: 50px;
  }

  .call-to-action h2 {
    font-size: 18px;
  }

  .call-to-action p {
    font-size: 14px;
  }

  .loading,
  .error {
    font-size: 14px;
  }
}

@media (max-width: 480px) {
  .searchbar {
    padding: 15px;
  }

  .app-download img {
    width: 80px;
  }

  th,
  td {
    font-size: 12px;
  }

  .content-box {
    box-shadow: none;
    border: none;
  }

  .action-button {
    padding: 6px;
    font-size: 12px;
  }

  h2 {
    font-size: 16px;
  }
}
</style>
