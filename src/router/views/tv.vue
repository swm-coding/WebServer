<script>
/* eslint-disable */
import appConfig from "@src/app.config";
import Layout from "@layouts/main";
import $ from "jquery";
import design from "@design";
require("@design");



const ERROR_TYPE = {
  VALUE: 1,
  INTERVAL: 2,
  MIN: 3,
  MAX: 4,
  ORDER: 5
};


export default {
  page: {
    title: "Dashboard",
    meta: [
      { name: "description", content: appConfig.description },
      { name: "Access-Control-Allow-Origin", content: "http://localhost:8080" },
      { name: "Access-Control-Allow-Credentials", content: true },
      { name: "Access-Control-Allow-Methods", content: "GET,PUT,POST,DELETE" },
      { "Access-Control-Allow-Origin": "http://localhost:8080" },
      { "Access-Control-Allow-Credentials": true },
      { "Access-Control-Allow-Methods": "GET,PUT,POST,DELETE" }
    ]
  },
  components: { Layout },
  data() {
    return {
      priceSlider: {
        value: [1, 500],
        min: 1,
        max: 1000,
        errorMsg: ""
      },
      displaySlider: {
        value: [20, 120],
        min: 1,
        max: 200,
        errorMsg: ""
      },
      errorMsg: "",
      title: "Smartphone",
      contents: [],
      active: [],
      count: 0,
      page: 1
    };
  },
  methods: {
    clearErrorMsgDisplay: function() {
      this.displaySlider.errorMsg = "";
    },
    clearErrorMsgPrice: function() {
      this.priceSlider.errorMsg = "";
    },
    error(type, msg) {
      switch (type) {
        case ERROR_TYPE.MIN:
          break;
        case ERROR_TYPE.MAX:
          break;
        case ERROR_TYPE.VALUE:
          break;
      }
      this.errorMsg = msg;
    },
    ignoreDisplayFalse: function() {
      var chkDisplay = document.getElementById('ignoreDisplay')
      chkDisplay.checked = false
      // this.isIgnoreDisplay = false
    },
    ignorePriceFalse: function() {
      var chkPrice = document.getElementById('ignorePrice')
      chkPrice.checked = false
      // this.isIgnorePrice = false
    },
    check: function(callback) {
      var chk,i;
      var data = {};
      data.company = [];
      data.display = [this.displaySlider.value[0],this.displaySlider.value[1]]
      data.price = [this.priceSlider.value[0], this.priceSlider.value[1]]

      chk = document.getElementsByName('company')
      for( i = 0 ; i < chk.length ; i++){
        if(chk[i].checked === true) data.company.push(chk[i].id)
      }
      var chkDisplay = document.getElementById('ignoreDisplay')
      var chkPrice = document.getElementById('ignorePrice')
      data.isIgnoreDisplay = chkDisplay.checked
      data.isIgnorePrice = chkPrice.checked
      console.log(data)
      $.ajax({
        type: "GET",
        data: data,
        contentType: "application/json",
        url: "http://52.78.210.26:8080/tv",
        success: res => {
          console.log("/tv..");
          this.contents = [];
          for (var i = 0; i < res.length; i++) {
            this.contents.push(res[i]);
          }
          this.count = Math.ceil(this.contents.length / 30);
          console.log(this.count);
          console.log(res);
          if (callback) callback(res);
        }
      });
    },
    load: function() {
      this.check(res => {
        this.contents = [];
        for (var i = 0; i < res.length; i++) {
          if (res[i].price.length === 1) res[i].price = 0;
          res[i].time = res[i].time.substring(0, 10);
          res[i].price = new Intl.NumberFormat('ko-KR', { style: 'currency', currency: 'KRW' }).format(res[i].price)
          if(res[i].site == 1) res[i].site = "중고나라"
          else if(res[i].site == 2) res[i].site = "번개장터"
          else if(res[i].site == 3) res[i].site = "당근마켓"
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
        if (res[i].price.length === 1) res[i].price = 0;
        res[i].time = res[i].time.substring(0, 10);
        res[i].price = new Intl.NumberFormat('ko-KR', { style: 'currency', currency: 'KRW' }).format(res[i].price)
        if(res[i].site == 1) res[i].site = "중고나라"
          else if(res[i].site == 2) res[i].site = "번개장터"
          else if(res[i].site == 3) res[i].site = "당근마켓"
        this.contents.push(res[i]);
        this.active[i] = false;
      }
    });
  }
};
</script>

<template>
  <Layout>
    <div class="row">
      <div class="col-12">
        <div class="page-title-box">
          <div class="page-title-right">
            <ol class="breadcrumb m-0">
              <li class="breadcrumb-item">
                <a href="javascript: void(0);">가전제품</a>
              </li>
              <li class="breadcrumb-item active">TV</li>
            </ol>
          </div>
          <h4 class="page-title">TV</h4>
        </div>
      </div>
    </div>
    <!-- end page title -->
    <div class="row">
      <div class="col-12">
        <div class="card-box">
          <div class="row">
            <label class="col-sm-3 col-form-label">제조사</label>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="삼성" type="checkbox" name="company" />
              <label for="company_checkbox0">삼성</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="LG" type="checkbox" name="company" />
              <label for="company_checkbox1">LG</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="금영" type="checkbox" name="company" />
              <label for="company_checkbox2">금영</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="대우" type="checkbox" name="company" />
              <label for="company_checkbox3">대우</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="샤오미" type="checkbox" name="company" />
              <label for="company_checkbox4">샤오미</label>
            </div>
          </div>


          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label">디스플레이</label>
            <div class="col-sm-1 ">
              <input id="ignoreDisplay" type="checkbox" name="ignore" @click="load" Checked>무시</input>
            </div>
            <div class="col-sm-1 ">
              <input class="form-control form-control-sm" v-model="displaySlider.value[0]" @input="clearErrorMsgDisplay" />
            </div>
            <div class="col-sm-6">
            <vue-slider
              v-model="displaySlider.value"
              :min="displaySlider.min"
              :max="displaySlider.max"
              :tooltip="displaySlider.errorMsg ? 'none' : 'always'"
              :marks="[displaySlider.min, displaySlider.max]"
              @error="error"
              @change="clearErrorMsgDisplay"
              @drag-end="load"
              @drag-start="ignorePriceFalse"
            ></vue-slider>
            </div>
            <div class="col-sm-1">
              <input class="form-control form-control-sm" v-model="displaySlider.value[1]" @input="clearErrorMsgDisplay" />
            </div>
          </div>


          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label">가격(만원)</label>
            <div class="col-sm-1 ">
              <input id="ignorePrice" type="checkbox" name="ignore" @click="load" Checked>무시</input>
            </div>
            <div class="col-sm-1 ">
              <input class="form-control form-control-sm" v-model="priceSlider.value[0]" @input="clearErrorMsgPrice" />
            </div>
            <div class="col-sm-6">
            <vue-slider
              v-model="priceSlider.value"
              :min="priceSlider.min"
              :max="priceSlider.max"
              :tooltip="priceSlider.errorMsg ? 'none' : 'always'"
              :marks="[priceSlider.min, priceSlider.max]"
              @error="error"
              @change="clearErrorMsgPrice"
              @drag-end="load"
              @drag-start="ignorePriceFalse"
            ></vue-slider>
            </div>
            <div class="col-sm-1">
              <input class="form-control form-control-sm" v-model="priceSlider.value[1]" @input="clearErrorMsgPrice" />
            </div>
          </div>
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
                        <th data-priority="1" id="tech-companies-1-col-3">사이트</th>
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
                        <!-- <button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="bottom" title="" data-original-title="Tooltip on bottom">Tooltip on bottom</button> -->
                        <!-- <th class="submenu" data-org-colspan="1" data-columns="tech-companies-1-col-0">{{con.title}}content..~~~</th> -->
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
                        <td
                          data-org-colspan="1"
                          data-priority="1"
                          data-columns="tech-companies-1-col-1"
                        >{{con.site}}</td>
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

<style lang="scss" module>
</style>