<template>
  <div id="app">
    <nav>Client Records</nav>
    <div class="filter">
      <div id="search">
        <div class="search-type">
          <b class="search-field">Search</b>
          <input name="query" placeholder="Name/Email/Company" v-model="searchQuery" />
        </div>
        <div class="search-type">
          <b class="search-field">From</b>
          <input type="date" v-model="startDate" />
        </div>
        <div class="search-type">
          <b class="search-field">To</b>
          <input type="date" v-model="endDate" />
        </div>
      </div>
    </div>
    <main>
      <div class="vld-parent">
        <loading :active.sync="isLoading"></loading>

        <Table
          v-if="tableData"
          :childData="filteredTableData"
          :config="config"
          :style="{maxHeight: '495px'}"
        />
      </div>
    </main>
    <div class="footer">
      <p>Copyright © 2020 All rights reserved.</p>
    </div>
  </div>
</template>

<script>
import Table from "./components/Table";
import moment from "moment";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";

export default {
  components: {
    Table,
    Loading,
  },
  data: function () {
    return {
      tableData: undefined,
      searchQuery: "",
      startDate: "",
      endDate: "",
      isLoading: true,
      fullPage: true,
      config: [
        {
          key: "name",
          title: "Name",
        },
        {
          key: "email",
          title: "Email",
        },
        {
          key: "company",
          title: "Company",
        },
        {
          key: "createdAt",
          title: "Created Date",
          type: "date",
        },
      ],
    };
  },
  computed: {
    filteredTableData() {
      var fDate, lDate, tab;
      if (this.searchQuery) {
        var table = this.tableData;
        var filterKey = this.searchQuery && this.searchQuery.toLowerCase();
        table = table.filter(function (row) {
          return Object.keys(row).some(function (key) {
            return String(row[key]).toLowerCase().indexOf(filterKey)>-1;
          });
        });
        if (this.startDate && this.endDate) {
          fDate = moment(this.startDate);
          lDate = moment(this.endDate);
          tab = table.filter(function (row) {
            var compareDate = moment(row["createdAt"]);
            var comparison = moment(compareDate).isBetween(fDate, lDate);
            if (comparison) {
              return true;
            } else {
              return false;
            }
          });
          return tab;
        } else {
          return table;
        }
      } else if (this.startDate && this.endDate) {
        tab = this.tableData;
        fDate = moment(this.startDate);
        lDate = moment(this.endDate);
        tab = tab.filter(function (row) {
          var compareDate = moment(row["createdAt"]);
          var comparison = moment(compareDate).isBetween(fDate, lDate);
          if (comparison) {
            return true;
          } else {
            return false;
          }
        });
        return tab;
      } else {
        return this.tableData;
      }
    },
  },

  mounted() {
    this.$axios
      .get("https://5f40a2dea5e9db0016301f4a.mockapi.io/assignment/users")
      .then(({ data }) => {
        this.tableData = data;
        this.isLoading = false;
      });
  },
};
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
  font-weight: 400;
  margin: 0;
}
nav {
  height: 60px;
  background: #eda806;
  font-size: 32px;
  color: black;
  display: flex;
  align-items: center;
  padding-left: 20px;
}
#search {
  text-align: center;
  margin: 2em 0;
}
.search-type {
  display: inline-block;
}
.search-field {
  margin: 0 1em;
}
.footer {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  background-color: #292929;
  color: white;
  text-align: center;
}
</style>