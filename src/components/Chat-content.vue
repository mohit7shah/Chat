<template>
  <div class="chathome">
    <div class="functionality">
      <div class="search col-4">
        <b-form-input
          :id="`type-date`"
          type="date"
          placeholder="yyyy-mm-dd"
          v-model.trim="query"
          @keyup.enter="search"
        ></b-form-input>
      </div>
      <div class="searchText col-4">
        <b-form-input
          :id="`type-msg-${index}`"
          type="text"
          placeholder="Search message"
          v-model.trim="msg"
          @keyup.enter="searchMsg"
          class="searchtext"
        ></b-form-input>
      </div>
      <div class="mr-auto">
        <b-dropdown id="dropdown1" right text="Sorting by Date" class="m-md-2">
          <b-dropdown-item @click="asc">Ascending</b-dropdown-item>
          <b-dropdown-item @click="desc">Descending</b-dropdown-item>
        </b-dropdown>
      </div>
    </div>
    <div class="chat">
      <b-card v-for="(details, index) in data" :key="index" class="main_card">
        <div class="row">
          <div class="col-3">
            <b-card class="color"
              ><h5>
                <p>date : {{ details.date }}</p>
              </h5>
            </b-card>
          </div>
          <div class="col-3">
            <b-card class="color">
              <h5>from : {{ details.value.email }}</h5>
            </b-card>
          </div>
          <div class="col-3"></div>
          <div class="col-3">
            <div class="newadding">
              <b-dropdown
                id="dropdown1"
                right
                text="Sorting by time"
                class="m-md-2"
              >
                <b-dropdown-item @click="asctime(index)"
                  >Ascending</b-dropdown-item
                >
                <b-dropdown-item @click="desctime(index)"
                  >Descending</b-dropdown-item
                >
              </b-dropdown>
            </div>
          </div>
        </div>

        <div class="row">
          <b-card
            v-for="(user, ind) in details.value.to"
            :key="ind"
            class="child_card col-sm-4"
          >
            <h5>to :</h5>
            <div class="tab4">
              <b>name: {{ user.user }}</b>
              <br />
              <b>message: {{ user.msg }}</b>
              <br />
              <b>time: {{ user.time }}</b>
            </div>
          </b-card>
        </div>
        <hr />
      </b-card>
    </div>
  </div>
</template>

<script>
import datadetails from "../Data.json";
export default {
  data() {
    return {
      msg: "",
      query: "",
      data: datadetails,
    };
  },
  methods: {
    search() {
      let query = this.query;
      let data = this.data;
      let result = data.filter((item) => {
        return item.date.includes(query);
      });
      this.data = result;
    },
    searchMsg() {
      this.data.forEach((item) => {
        let tempChildData = [];

        item.value.to.forEach((user) => {
          if (user.msg.includes(this.msg)) {
            tempChildData.push(user);
          }
        });
        item.value.to = tempChildData;
      });
    },
    asc() {
      let data = this.data;
      let result = data.sort(function (a, b) {
        return new Date(a.date) - new Date(b.date);
      });
      this.data = result;
    },
    desc() {
      let data = this.data;
      let result = data.sort((a, b) => {
        return new Date(b.date) - new Date(a.date);
      });
      this.data = result;
    },
    asctime(index) {
      // console.log(this.data);
      let data = this.data[index].value.to;
      // console.log(data);
      const getNumber = (t) => +t.replace(/:/g, "");
      data.sort((a, b) => {
        // console.log(a);
        return getNumber(a.time) - getNumber(b.time);
      });
      console.log(data);
      this.data[index].value.to = data;
    },
    desctime(index) {
      let data = this.data[index].value.to;
      const getNumber = (t) => +t.replace(/:/g, "");
      data.sort((a, b) => {
        return getNumber(b.time) - getNumber(a.time);
      });
      this.data[index].value.to = data;
    },
  },
};
</script>

<style scoped>
.search {
  margin-top: 10px;
  margin-left: 10px;
  display: flex;
  width: 20%;
}

.searchtext {
  display: flex;
  width: 100%;
  margin: 10px;
}

.mr-auto {
  margin-right: 3px;
}

.newadding {
  display: flex;
  width: 100%;
  justify-content: space-between;
}

.functionality {
  justify-content: space-between;
  display: flex;
  width: 100%;
}

.main_card {
  /* padding: 10px; */
  margin-top: 10px;
  border: none;
  border: 1px solid #ccc;
  border-radius: 10px;
}

.child_card {
  width: 32%;
  display: flex;
  flex-grow: inherit;
  margin-top: 10px;
  margin-left: 15px;
  height: 150px;
  box-shadow: 0px 0px 5px grey;
}

.tab4 {
  margin-left: 38px;
}

.color {
  background: rgb(228, 228, 228);
  border-radius: 5px;
  text-align: center;
  border: none;
  height: 50px;
}
.chat {
  margin: 10px 10px 50px 10px;
  border: none;
  text-decoration: none;
  /* box-shadow: 0px 0px 5px black; */
}

.change {
  background: lightgreen;
  border-radius: 5px;
  padding-left: 10px;
}
</style>
