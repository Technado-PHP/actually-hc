<template>
  <div class="discussion">
    <div class="container">
      <div class="text-right si-parent">
        <div class="si-wrap">
        <input class="search-input" type="text" placeholder="Search comments" v-model="words">
        <font-awesome-icon :icon="['fas', 'search']" />
        </div>
      </div>
      <div class="question-wrap">
        <div class="mb-3">
          <router-link to="/categories" class="goto-btn"
            >Go to Health Categories</router-link
          >
        </div>
        <h2>{{ categoryName }}</h2>
      </div>
      <div class="comments-link-wrap">
        <Comments
          :words="words"
          :commentss="commentsData"
          :discuss_id="discuss_id"
          @commented="newCommentHappened"
        />
        
      </div>
    </div>
  </div>
</template>

<script>
import Comments from "@/components/Comments.vue";

export default {
  name: "Discussion",
  components: { Comments },
  data() {
    return {
      data: [],
      commentsData: [],
      categoryName: "",
      discuss_id: null,
      searchQuery: 'Lorem ipsum dolor sit amet',
      words: ''
    };
  },
  methods: {
    // highlightKeyword: function(str) {
    //   if(str && this.searchQuery) {
    //       var highlight = [this.searchQuery.trim(), this.searchQuery.toLowerCase().trim()];
    //       str = ' ' + str;
    //       return str.replace(new RegExp('(.)(' + highlight.join('|') + ')(.)','ig'), '$1<span class="highlight">$2</span class="highlight">$3');
    //   }
    //   else return str;
    // },
    newCommentHappened() {
      const data = new FormData();
      data.append("register_id", localStorage.getItem("UserID"));
      data.append("discus_id", this.discuss_id);
      require("axios")
        .post(process.env.VUE_APP_APIURL + "discussions_comments_api", data, {
          headers: {
            token: localStorage.getItem("UserToken"),
          },
        })
        .then((response) => {
          this.data = response.data.Message.Discussion,
          this.commentsData = response.data.Message.Comments;
        })
        .catch((error) => {
          console.log("Error", error);
        });
    },
  },
  created() {
    require("axios")
      .get(process.env.VUE_APP_APIURL + "discussion_api/" + this.$route.params.id)
      .then((response) => {
        if (response.data.Message.Discussion) {
          const discuss_id = [...response.data.Message.Discussion][0].DiscussId;
          const data = new FormData();
          data.append("register_id", localStorage.getItem("UserID"));
          data.append("discus_id", discuss_id);
          require("axios")
            .post(
              process.env.VUE_APP_APIURL + "discussions_comments_api", data, 
              {
                headers: {
                  token: localStorage.getItem("UserToken"),
                },
              }
            )
            .then((response) => {
              this.data = response.data.Message.Discussion,
              this.discuss_id = discuss_id;
              this.commentsData = response.data.Message.Comments;
              this.categoryName = this.data[0].CategoryName;
            })
            .then(() => {
              require('jquery')('.summernote').summernote({
                toolbar: [
                  ['fontstyle', ['fontname', 'fontsize', 'color', 'bold', 'italic', 'underline', 'strikethrough', 'superscript', 'subscript', 'deleteallformat']],
                  ['insert', ['link']],
                  ['para', ['style', 'ul', 'ol', 'paragraph']],
                ],
                height: 100,
              });
            })
            .catch((error) => {
              console.log("Error", error);
            });
        } else {
          alert("Nothing to discuss.");
        }
      })
      .catch((error) => {
        console.log("Error", error);
      });
  },
};
</script>

<style scoped>
.discussion {
  padding-top: 30px;
  background: #ecf0f1;
}
.question-wrap {
  padding: 30px 0;
}
.si-parent{
  display: flex;
  justify-content: flex-end;
}
.si-wrap{
  display: flex;
  align-items: center;
  justify-content: flex-end;
  border: 1px solid #e8e8e8;
  padding: 5px;
  border-radius: 3px;
  background: #f4f4f4;
}
.search-input{
  background: transparent;
  border: none;
  min-width: 300px;
}
.question-wrap h2 {
  font-size: 24px;
  font-weight: 700;
  color: #000;
  line-height: 1.2;
  margin-bottom: 25px;
  text-transform: uppercase;
  position: relative;
}
.question-wrap h2::before {
  content: "";
  position: absolute;
  height: 3px;
  width: 30px;
  background-image: linear-gradient(
    to top,
    #ac0000,
    #c00000,
    #d40000,
    #e80000,
    #fd0000
  );
  bottom: -4px;
  left: 0;
}
.question-wrap h3 {
  font-size: 18px;
  font-weight: 500;
  color: #282828;
  line-height: 1.5;
  font-family: Poppins, sans-serif;
  padding: 20px;
}
.question-inner {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  background-color: #ffffff;
  border-radius: 2px;
  box-shadow: 0 1px 2px #c9cccd;
}
.quest-right-wrap {
  min-width: 100px;
  text-align: center;
}
.ques-comments {
  padding: 20px 0;
  border-left: 1px solid #e0e0e0;
}
.commentbg {
  background-color: #bdc3c7;
  border-radius: 2px;
  display: inline-block;
  padding: 7px 17px;
  color: #ffffff;
  font-size: 14px;
  position: relative;
}
.commentbg::after {
  content: "";
  width: 11px;
  height: 11px;
  background-color: #bdc3c7;
  position: absolute;
  bottom: 0;
  left: 43%;
  margin-bottom: -5px;
  transform: rotate(45deg);
}
.ques-time {
  color: #282828;
  font-size: 12px;
  line-height: 29px;
  padding: 0 10px;
  border-left: 1px solid #e0e0e0;
  border-top: 1px solid #e0e0e0;
}
.iconn {
}
.ques-time span {
  margin-left: 5px;
}
.comments-link-wrap {
  display: flex;
  justify-content: space-between;
}
.goto-btn {
  display: inline-block;
  width: 230px;
  background-image: linear-gradient(
    to top,
    #ac0000,
    #c00000,
    #d40000,
    #e80000,
    #fd0000
  );
  color: #fff;
  font-family: "Poppins", sans-serif;
  font-size: 16px;
  line-height: 26px;
  font-weight: 700;
  height: 37px;
  border: none;
  text-align: center;
  padding: 5px 0;
}

@media only screen and (max-width: 575px) {
  .question-wrap h3 {
    font-size: 18px;
    font-weight: 500;
    color: #282828;
    line-height: 1.5;
    font-family: Poppins, sans-serif;
    padding: 10px;
  }
  .ques-comments {
    padding: 15px 0;
  }
  .comments-link-wrap {
    flex-wrap: wrap;
  }
  .comments {
    order: 2;
    margin-top: 10px;
  }
}
</style>