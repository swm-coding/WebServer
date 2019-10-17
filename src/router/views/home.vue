<script>
import appConfig from "@src/app.config";
import Layout from "@layouts/main";
import $ from "jquery";

export default {
  page: {
    title: "Dashboard",
    meta: [{ name: "description", content: appConfig.description }]
  },
  components: { Layout },
  data() {
    return {
      title: "Home",
      contents: [],
      active: [],
      count: 0,
      page: 1
    };
  },
  methods: {
    check: function(callback) {
      var data = {};
      console.log(data);
      $.ajax({
        type: "GET",
        data: data,
        contentType: "application/json",
        url: "http://52.78.210.26:8080/",
        success: res => {
          this.contents = [];
          for (var i = 0; i < res.length; i++) {
            this.contents.push(res[i]);
          }
          this.count = Math.ceil(this.contents.length / 30);
          console.log(this.count);
          console.log(res);
          var result = [];
          for (i = 0; i < res.length; i++) {
            console.log(res[i]);
            console.log(res[i].length);
            for (var j = 0; j < res[i].length; j++) {
              result.push(res[i][j]);
              console.log(res[i][j]);
            }
          }
          console.log(result);

          if (callback) callback(result);
        }
      });
    },
    load: function() {
      this.check(res => {
        this.contents = [];
        for (var i = 0; i < res.length; i++) {
          if (res[i].price.length === 1) res[i].price = "0원";
          res[i].time = res[i].time.substring(0, 10);
          this.contents.push(res[i]);
          this.active[i] = false;
        }
      });
    }
  },
  mounted() {
    // this.load()
    this.check(res => {
      this.contents = [];
      for (var i = 0; i < res.length; i++) {
        if (res[i].price.length === 1) res[i].price = "0원";
        res[i].time = res[i].time.substring(0, 10);
        this.contents.push(res[i]);
        this.active[i] = false;
      }
    });
  }
};
</script>

<template>
  <Layout>
    <!-- 
    <div class="row">
      <div class="col-12">
        <div class="page-title-box">
          <div class="page-title-right">
            <ol class="breadcrumb m-0">
              <li class="breadcrumb-item"><a href="javascript: void(0);">Adminto</a></li>
              <li class="breadcrumb-item active">Dashboard</li>
            </ol>
          </div>
          <h4 class="page-title">Dashboard</h4>
        </div>
      </div>
    </div>-->
    <!-- end page title -->
    <!-- <div class="row">
      <div class="col-md-12">
        <b-card>
          This is dashboard page
        </b-card>
      </div>
    </div>-->

    <div class="row">
      <div class="col-12">
        <div class="page-title-box">
          <div class="page-title-right">
            <ol class="breadcrumb m-0">
              <li class="breadcrumb-item">
                <a href="javascript: void(0);">Home</a>
              </li>
            </ol>
          </div>
          <h4 class="page-title">Home</h4>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col-12">
        <div class="card-box">
          <!-- 상품 정보 표기 -->
          <div class="responsive-table-plugin">
            <div class="table-wrapper">
              <div class="table-rep-plugin fixed-solution" data-pattern="priority-columns">
                <div class="table-responsive" data-pattern="priority-columns">
                  <table id="tech-companies-1" class="table table-striped mb-0">
                    <thead>
                      <tr>
                        <th id="tech-companies-1-col-0-clone">글 제목</th>
                        <th data-priority="1" id="tech-companies-1-col-1">가격</th>
                        <th data-priority="1" id="tech-companies-1-col-2">날짜</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="(con) in contents" v-bind:key="con._id">
                        <th data-org-colspan="1" data-columns="tech-companies-1-col-0">
                          <a
                            :href="con.url"
                            data-toggle="toggle"
                            data-placement="top"
                            :title="con.title"
                          >{{con.title}}</a>
                        </th>
                        <td
                          data-org-colspan="1"
                          data-priority="1"
                          data-columns="tech-companies-1-col-1"
                        >{{con.price}}</td>
                        <td
                          data-org-colspan="1"
                          data-priority="1"
                          data-columns="tech-companies-1-col-1"
                        >{{con.time}}</td>
                      </tr>
                    </tbody>
                  </table>
                  <div id="tech-companies-1" class="table table-striped mb-0"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Layout>
</template>
