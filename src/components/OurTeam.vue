<template>
  <section class="team-sec">
    <div class="container">
      <div class="row justify-content-center">
        <div
          class="col-sm-12 col-md-12 col-lg-6 mb-5"
          v-for="(member, index) in members"
          :key="index"
        >
          <div class="single-member-wrap">
            <div class="sm-img-wrap">
              <img :src="baseURL + member.TeamMemberImage" class="img-fluid" alt="" />
              <div class="sm-img-inner">
                <h3>{{ member.TeamMemberName }}</h3>
                <h5>{{ member.TeamMemberDesignation }}</h5>
              </div>
            </div>
            <div class="sm-desc">
              <p v-html="member.TeamText"></p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "OurTeam",
  data() {
    return {
      axios: require("axios"),
      baseURL: process.env.VUE_APP_APIURL + "uploads/team/",
      members: [],
    };
  },
  mounted() {
    console.log(process.env.VUE_APP_APIURL);
    this.axios
      .get(process.env.VUE_APP_APIURL + "teampage_api")
      .then((response) => {
        this.members = [...response.data.Message.Team];
      })
      .catch((error) => {
        console.log("Error", error);
      });
  },
};
</script>

<style scoped>
.team-sec {
  padding: 30px 0;
}
.single-member-wrap {
  background: #f4f4f4;
  height: 100%;
  border: 1px solid #efefef;
  border-radius: 5px;
}
.sm-img-wrap {
  display: flex;
  align-items: center;
}
.sm-img-wrap img {
  max-width: 170px;
}
.sm-img-inner {
  margin-left: 20px;
}
.sm-desc {
}
.sm-img-wrap h3 {
  font-family: "Poppins", sans-serif;
  font-size: 22px;
  font-weight: 600;
  color: #b90000;
  line-height: 1.2;
}
.sm-img-wrap h5 {
  font-family: "Poppins", sans-serif;
  font-size: 18px;
  font-weight: 500;
  color: #000;
  line-height: 1.2;
}
.sm-desc p {
  color: #000;
  font-size: 14px;
  padding: 25px;
  line-height: 2;
}

@media only screen and (max-width: 575px) {
  .sm-img-inner {
    margin-left: 15px;
  }
  .sm-img-wrap h5 {
    font-size: 16px;
  }
  .sm-desc p {
    padding: 20px 10px;
  }
  .sm-img-wrap {
    display: block;
    text-align: center;
  }
}
</style>
