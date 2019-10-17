<script>
/* eslint-disable */
import appConfig from "@src/app.config"
import Layout from "@layouts/main"
import $ from "jquery"
// import BootstrapVue from 'bootstrap-vue'
require("@design")


$(document).ready(function(){
  var count = 1
  $('#search-box').keydown(function(event) {
    if (event.keyCode === 13 ){
      $('.btn-box').click()
    }
  })
  $(window).scroll(function() {
    var scrollHeight = $(window).scrollTop() + $(window).height()
    var documentHeight = $(document).height();

    if(scrollHeight == documentHeight){
      console.log("++++++++")
      load
    }
  })
});

const ERROR_TYPE = {
  VALUE: 1,
  INTERVAL: 2,
  MIN: 3,
  MAX: 4,
  ORDER: 5
};

// import design from "@design";
// require("@design");
// import './ion.rangeSlider'

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
  data () {
    return {
      displaySlider: {
        value: [11, 16],
        min: 9,
        max: 20,
        errorMsg: ""
      },
      ramSlider: {
        value: [4, 16],
        min: 1,
        max: 64,
        errorMsg: ""
      },
      ssdSlider: {
        value: [128, 512],
        min: 32,
        max: 1024,
        errorMsg: ""
      },
      priceSlider: {
        value: [1, 500],
        min: 0,
        max: 1000,
        errorMsg: ""
      },
      title: "Laptop",
      contents: [],
      active: [],
      count: 0,
      page: 1,
      pagerange: [],
      isIgnoreRam: true,
      isIgnoreSSD: true,
      isIgnoreDisplay: true,
      isIgnorePrice: true
    };
  },
  mounted () {
    $(document).ready(function(){
      $('[data-toggle="popover"]').popover();
    });
    // this.load()
    if(this.page <= 3){
      this.pagerange = [1,2,3,4,5]
    }
    else{
      this.pagerange = [this.page-2,this.page-1,this.page,this.page+1,this.page+2]
    }

    this.check(res => {
      this.contents = [];
      for (var i = 0; i < res.length; i++) {
        if (res[i].price.length === 1) res[i].price = "0원";
        res[i].time = res[i].time.substring(0, 10);
        res[i].price = new Intl.NumberFormat('ko-KR', { style: 'currency', currency: 'KRW' }).format(res[i].price)
        if(res[i].site == 1) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAPCAMAAAAMCGV4AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAACW1BMVEUAAADsZSzmYivwZy3yZy3yZy3yZy3yZy3yZy3xZyzuZSzuZSzzaC3vZizzaC30aC30aC30aC30aC30aC3zaC3jYyvvZizlYirvZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizvZizuZizuZizsZSzsZSzsZSzsZSzsZSzsZSzsZSzsZSzsZSztZizuZizvZiztZSzuZizzaC30aC3wZyzwZyy7UiPsZSzsZSz4ai7xZy0AAADgYSztZizwZy3tZSzvZizzaC3vZizuZizuZizvZizvZizvZizvZizuZizuZizuZiztZSzwZy3tZSzuZizxZy3vZizvZizvZizvZizvZS7wZiztZSzsZSztZSzvZiztZSzwZy3tZSzvZizyZy3uZizuZizuZizvZiztZizvZizuZizuZiztZSzvZiztZSzwZy3tZSzuZiz0aC3wZizvZizvZizxZy3wZy3vZizwZy3wZizyZy3vZi3tZizwZy3uZizuZizuZizuZizuZizuZizuZizxZy3uZizuZizuZizuZizuZizuZizvZiz2aS7vZizuZizuZizuZizuZizuZizuZiztZSzuZizuZizuZizuZizuZizvZi3xZy3/cTHxZy3zaC3zaC3zaC3vZi3tZiztZSzxZy3zaC3zaC3xZy3xZy3wZi3wZy3vZizxZy3zaC30aC30aC3vZi3vZizvZizxZy3yaC30aC3yZy3vZizwZi3uZizxZyztZizyZy3wZy3wZyzpXCnvZizwZizxZy3wZy3wZyzsZSz///9QlC4tAAAAyHRSTlMAAAAAAAAAAAAAAAAAAAcNDQ0NDQoAAABFs8TExMTExMTEwHQLH8qVWllaXF1dXFpbcdhfPdQjBkAzAAAAAikAAKyPPtEpiLi/XwADLrQnBamQPtBAvidYqAE9vOu4OKeQPtEunqGzdQAMRcM+DamQPtIgFmdWCQAABkUDAKqRJ9NzMzM0LgkkNzQ1StFuAmPO09PT25zK3NLT1JYVAAAYJCQiR9X7YCAkHwYAAAAAAAAAADq3MwAAAAAAAAAADgQAAAAAAAAAAGt/iEYAAAABYktHRMgdulfKAAAACXBIWXMAAAsSAAALEgHS3X78AAAAB3RJTUUH4woPDTsREA2X4gAAAO1JREFUCNdjYGRiZmFlAwF2Dk4ubgYeXj5+AUEQEBIWERVjEJeQlJKWkZWTk1dQVFJWYVBVU9fQ1NLW0dXTNzA0MmYwMTUzt7C0sraxtbN3cHRicHZxdXP38PTy9vH18w8IZAgKDgkNC4+IjIqOiY2LT2BITEpOSU1Lz8jMys7JzctnKCgsKi4pLSuvqKyqrqmtY6hvaGxqbmlta+/o7Oru6WXo658wcdLkKVOnTZ8xc9bsOQxz581fsHDR4iVLly1fsXLVaoY1a9et37Bx0+YtW7dt37FzFwMY7N6zd9/+AwcZ4ODQ4SNHjx0HMgAaXU43wzbSkQAAACh0RVh0Q29tbWVudABDcm9wcGVkIHdpdGggZXpnaWYuY29tIEdJRiBtYWtlclmQRc0AAAAldEVYdGRhdGU6Y3JlYXRlADIwMTktMTAtMTVUMTM6NTk6MTMrMDA6MDAcTyAvAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDE5LTEwLTE1VDEzOjU5OjEzKzAwOjAwbRKYkwAAABJ0RVh0U29mdHdhcmUAZXpnaWYuY29toMOzWAAAAABJRU5ErkJggg=="
        else if(res[i].site == 2) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAPCAMAAAAMCGV4AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAACcFBMVEUAAACGAAD1AAD0AAD4AAD4AAD4AAD2AAD4AACUAAD0AAD/AAD4AAD6AAD4AAD4AAD3AADxAAD4AADsAADyAADmAAD4AAD3AAD4AAD3AAD3AAD3AAD3AAD3AAD3AAD4AAD6AAD3AAD7AADyAAD/AAD5AAD2AAD3AAD3AAD3AAD3AAD3AAD3AAD3AAD3AAD4AAD3AAD7AAD/AAD5AAD9AAD4AAD3AAD3AAD3AAD5ODj6cnL4ICD3AAD3AAD4AAD5AAD5AAD9AAD/AAD4AAD3AAD3AAD3AgL4ISH3AAD3AAD4AAD7AAD7AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD3AAD3AAD3AAD4AAD7AAD8AAD5AAD8AAD4AAD3AAD3AAD3AQH6bm73AAD3AAD3AAD3AAD4AAD6AAD6AAD/AAD6AAD2AAD4AAD3AAD3AQH3EBD3AAD3AAD3AAD3AAD3AAD4AAD6AADaAADZAAD6AAD3AAD4AAD3AAD3AAD3AAD3AAD3AAD4AAD4AAD/AAD4AAD7AAD1AAD3AADxAAD4AAD0AAD4AAD3AAD3AAD3AAD4AAD0AAD1AAAIAAC7AAD3AAD2AAD4AAD/AAD5AAD3AAD4AADsAAD7d3f8pKT////+3d33AAD4GRn+1tb////8nJz3FBT3AgL3AAD5Rkb/+fn////+6Oj7hYX3BQX3AAD5ODj8oaH/+Pj9xcX4JSX3AAD5U1P9y8v4Li73AAD5QED///+C78H/AAAAsXRSTlMAAAAAAAAAAAAAAAAAACtyOQEAAAAAAAAAF3fe/+qLIgAAAAAAAApXyP7/////12sSAAAAAoj5/////////qsOAAAIvf///////+EeAAAIu///3x4AAAi7///fHgAACLv//98eAAAIvf//////4R4AAAOU/v////////+3EAAAABBq1v//////434aAAAAAAAAIYrp//GdLwAAAAAAAAABOYZIBQAAAAAAAAAAAAAAAACOt1tJAAAAAWJLR0TPg97CaQAAAAlwSFlzAAALEgAACxIB0t1+/AAAAAd0SU1FB+MKDw06CRp6PvUAAAD2SURBVAjXY2BgYGBkYmZhZWPn4GQAAy5uHl4+fgFBIWERIE9UTFxCUkpaRlZOXkFRSZlBRVVNXUNTS1tHV0/fwNCIwdjE1MzcwnKjlbWNrZ29A4Ojk7OLq9umzVvcPTy9vH0YfP38A7Zu275j567dgUHBIQyhYeERe/bu23/g4KHIqOgYhti4+ITDR44eO37iZGJScgpDalp6RmbWqdNnzmbn5OblMxQUFhWXlJadK6+orKquqWWoq29obGpuaW1r7+js6u5h6O3rnzBx0uQpU6dNnzFz1mygi+fMnTd/wcJFi5csXbYc7KEVK1etXrN23foNQDYAUNJTSiksT4cAAAAldEVYdGRhdGU6Y3JlYXRlADIwMTktMTAtMTVUMTM6NTc6NDMrMDA6MDBKZh74AAAAJXRFWHRkYXRlOm1vZGlmeQAyMDE5LTEwLTE1VDEzOjU3OjQzKzAwOjAwOzumRAAAAABJRU5ErkJggg=="
        else if(res[i].site == 3) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAPCAMAAADqIa48AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JQAAgIMAAPn/AACA6QAAdTAAAOpgAAA6mAAAF2+SX8VGAAABuVBMVEUAAAA3sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk33Zwc2s042s040s04zs04zs040s041s041s04wtVD3ZwfRdhWSjiyugyLSdhXYcxK3gB6ciijNdxb3Zwf3Zwf3Zwf3Zwf5Zgb4Zgf4Zwf5Zgb4Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf///9TLSPdAAAAknRSTlMAAAAEUmYPAAAAAC3m+1wAAAAAGpn9/7szAAAAAD3d19LmbAAAAAAKXouOcRoAAAAOje3///awJAAAff///vv//60OItr/5mtWy//1UETy/5MAAFz9/3897/+qCQB3//52Fcf/+K6d6//pOgBa9f//////igMABHf0///+oRIAAAADWNHieg4AAAAAACEtAAAAAGBlrhUAAAABYktHRJKWBO8gAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAB3RJTUUH4woPDToxMniGawAAAAFvck5UBlHG4jkAAACtSURBVAjXY2BgZGJmYWVj52BgYODk4ubh5eMXEGRgEBIWERUTl5CUYpCWkZWTV1BUUlZhUFVT19DU0tbR1WPQNzA0MjYxNTO3YLC0sraxtbN3cHRicHZxdXP38PTy9mHw9fMPCAwKDgkNYwiPiIyKjomNi09gSExKTklNS8/IzGLIzsnNyy8oLCouYSgtK6+orKquqa1jqG9obGpuaW1r72Bg6Ozq7unt658wEQDIwilal7IBjQAAAERlWElmTU0AKgAAAAgAAgESAAMAAAABAAYAAIdpAAQAAAABAAAAJgAAAAAAAqACAAQAAAABAAAAWKADAAQAAAABAAAAhQAAAABG/6etAAAAKHRFWHRDb21tZW50AENyb3BwZWQgd2l0aCBlemdpZi5jb20gR0lGIG1ha2VyWZBFzQAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAxOS0xMC0xNVQxMzo1ODo0MSswMDowMCzyVFwAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMTktMTAtMTVUMTM6NTg6NDErMDA6MDBdr+zgAAAAEnRFWHRleGlmOkV4aWZPZmZzZXQAMzituL4jAAAAF3RFWHRleGlmOlBpeGVsWERpbWVuc2lvbgA4OIuNV8IAAAAYdEVYdGV4aWY6UGl4ZWxZRGltZW5zaW9uADEzM2kZo2IAAAASdEVYdFNvZnR3YXJlAGV6Z2lmLmNvbaDDs1gAAAAASUVORK5CYII="
        this.contents.push(res[i]);
        this.active[i] = false;
      }
      var pageitems = document.getElementsByClassName('page-item')
      console.log(pageitems)
        for(var i in pageitems){
          var pagenum = pageitems[i].textContent
          var currentpage = String(this.page)
          if (pagenum == currentpage){
            pageitems[i].classList.add('active')
          }
        }
    });
  },
  methods: {
    clearErrorMsgRam: function() {
      this.ramSlider.errorMsg = "";
    },
    clearErrorMsgSSD: function() {
      this.ssdSlider.errorMsg = "";
    },
    clearErrorMsgDisplay: function() {
      this.displaySlider.errorMsg = "";
    },
    clearErrorMsgPrice: function() {
      this.priceSlider.errorMsg = "";
    },
    ignoreDisplayFalse: function() {
      var chkDisplay = document.getElementById('ignoreDisplay')
      chkDisplay.checked = true
    },
    ignoreRamFalse: function() {
      var chkRam = document.getElementById('ignoreRam')
      chkRam.checked = true
    },
    ignoreSSDFalse: function() {
      var chkSSD = document.getElementById('ignoreSSD')
      chkSSD.checked = true
    },
    ignorePriceFalse: function() {
      var chkPrice = document.getElementById('ignorePrice')
      chkPrice.checked = true
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
    check: function(callback) {
      var chk,i;
      var data = {};
      data.company = [];
      data.cpu = [];
      data.ram = [this.ramSlider.value[0],this.ramSlider.value[1]];
      data.display = [this.displaySlider.value[0],this.displaySlider.value[1]];
      data.ssd = [this.ssdSlider.value[0],this.ssdSlider.value[1]];
      data.price = [this.priceSlider.value[0]*10000,this.priceSlider.value[1]*10000];
      data.title = document.getElementById('search-box').value;

      chk = document.getElementsByName('company')
      for( i = 0 ; i < chk.length ; i++){
        if(chk[i].checked === true) data.company.push(chk[i].id)
      }
      chk = document.getElementsByName('cpu')
      for( i = 0 ; i < chk.length ; i++){
        if(chk[i].checked === true) data.cpu.push(chk[i].id)
      }
      var chkRam = document.getElementById('ignoreRam')
      var chkDisplay = document.getElementById('ignoreDisplay')
      var chkSSD = document.getElementById('ignoreSSD')
      var chkPrice = document.getElementById('ignorePrice')

      data.isIgnoreRam = !chkRam.checked
      data.isIgnoreSSD = !chkSSD.checked
      data.isIgnoreDisplay = !chkDisplay.checked
      data.isIgnorePrice = !chkPrice.checked
      console.log(data)
      $.ajax({
        type: "GET",
        data: data,
        contentType: "application/json",
        url: "http://52.78.210.26:8080/laptop",
        success: res => {
          console.log("/laptop..");
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
          if (res[i].price.length === 1) res[i].price = "0원";
          res[i].time = res[i].time.substring(0, 10);
          res[i].price = new Intl.NumberFormat('ko-KR', { style: 'currency', currency: 'KRW' }).format(res[i].price)
          if(res[i].site == 1) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAPCAMAAAAMCGV4AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAACW1BMVEUAAADsZSzmYivwZy3yZy3yZy3yZy3yZy3yZy3xZyzuZSzuZSzzaC3vZizzaC30aC30aC30aC30aC30aC3zaC3jYyvvZizlYirvZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizvZizuZizuZizsZSzsZSzsZSzsZSzsZSzsZSzsZSzsZSzsZSztZizuZizvZiztZSzuZizzaC30aC3wZyzwZyy7UiPsZSzsZSz4ai7xZy0AAADgYSztZizwZy3tZSzvZizzaC3vZizuZizuZizvZizvZizvZizvZizuZizuZizuZiztZSzwZy3tZSzuZizxZy3vZizvZizvZizvZizvZS7wZiztZSzsZSztZSzvZiztZSzwZy3tZSzvZizyZy3uZizuZizuZizvZiztZizvZizuZizuZiztZSzvZiztZSzwZy3tZSzuZiz0aC3wZizvZizvZizxZy3wZy3vZizwZy3wZizyZy3vZi3tZizwZy3uZizuZizuZizuZizuZizuZizuZizxZy3uZizuZizuZizuZizuZizuZizvZiz2aS7vZizuZizuZizuZizuZizuZizuZiztZSzuZizuZizuZizuZizuZizvZi3xZy3/cTHxZy3zaC3zaC3zaC3vZi3tZiztZSzxZy3zaC3zaC3xZy3xZy3wZi3wZy3vZizxZy3zaC30aC30aC3vZi3vZizvZizxZy3yaC30aC3yZy3vZizwZi3uZizxZyztZizyZy3wZy3wZyzpXCnvZizwZizxZy3wZy3wZyzsZSz///9QlC4tAAAAyHRSTlMAAAAAAAAAAAAAAAAAAAcNDQ0NDQoAAABFs8TExMTExMTEwHQLH8qVWllaXF1dXFpbcdhfPdQjBkAzAAAAAikAAKyPPtEpiLi/XwADLrQnBamQPtBAvidYqAE9vOu4OKeQPtEunqGzdQAMRcM+DamQPtIgFmdWCQAABkUDAKqRJ9NzMzM0LgkkNzQ1StFuAmPO09PT25zK3NLT1JYVAAAYJCQiR9X7YCAkHwYAAAAAAAAAADq3MwAAAAAAAAAADgQAAAAAAAAAAGt/iEYAAAABYktHRMgdulfKAAAACXBIWXMAAAsSAAALEgHS3X78AAAAB3RJTUUH4woPDTsREA2X4gAAAO1JREFUCNdjYGRiZmFlAwF2Dk4ubgYeXj5+AUEQEBIWERVjEJeQlJKWkZWTk1dQVFJWYVBVU9fQ1NLW0dXTNzA0MmYwMTUzt7C0sraxtbN3cHRicHZxdXP38PTy9vH18w8IZAgKDgkNC4+IjIqOiY2LT2BITEpOSU1Lz8jMys7JzctnKCgsKi4pLSuvqKyqrqmtY6hvaGxqbmlta+/o7Oru6WXo658wcdLkKVOnTZ8xc9bsOQxz581fsHDR4iVLly1fsXLVaoY1a9et37Bx0+YtW7dt37FzFwMY7N6zd9/+AwcZ4ODQ4SNHjx0HMgAaXU43wzbSkQAAACh0RVh0Q29tbWVudABDcm9wcGVkIHdpdGggZXpnaWYuY29tIEdJRiBtYWtlclmQRc0AAAAldEVYdGRhdGU6Y3JlYXRlADIwMTktMTAtMTVUMTM6NTk6MTMrMDA6MDAcTyAvAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDE5LTEwLTE1VDEzOjU5OjEzKzAwOjAwbRKYkwAAABJ0RVh0U29mdHdhcmUAZXpnaWYuY29toMOzWAAAAABJRU5ErkJggg=="
          else if(res[i].site == 2) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAPCAMAAAAMCGV4AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAACcFBMVEUAAACGAAD1AAD0AAD4AAD4AAD4AAD2AAD4AACUAAD0AAD/AAD4AAD6AAD4AAD4AAD3AADxAAD4AADsAADyAADmAAD4AAD3AAD4AAD3AAD3AAD3AAD3AAD3AAD3AAD4AAD6AAD3AAD7AADyAAD/AAD5AAD2AAD3AAD3AAD3AAD3AAD3AAD3AAD3AAD3AAD4AAD3AAD7AAD/AAD5AAD9AAD4AAD3AAD3AAD3AAD5ODj6cnL4ICD3AAD3AAD4AAD5AAD5AAD9AAD/AAD4AAD3AAD3AAD3AgL4ISH3AAD3AAD4AAD7AAD7AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD3AAD3AAD3AAD4AAD7AAD8AAD5AAD8AAD4AAD3AAD3AAD3AQH6bm73AAD3AAD3AAD3AAD4AAD6AAD6AAD/AAD6AAD2AAD4AAD3AAD3AQH3EBD3AAD3AAD3AAD3AAD3AAD4AAD6AADaAADZAAD6AAD3AAD4AAD3AAD3AAD3AAD3AAD3AAD4AAD4AAD/AAD4AAD7AAD1AAD3AADxAAD4AAD0AAD4AAD3AAD3AAD3AAD4AAD0AAD1AAAIAAC7AAD3AAD2AAD4AAD/AAD5AAD3AAD4AADsAAD7d3f8pKT////+3d33AAD4GRn+1tb////8nJz3FBT3AgL3AAD5Rkb/+fn////+6Oj7hYX3BQX3AAD5ODj8oaH/+Pj9xcX4JSX3AAD5U1P9y8v4Li73AAD5QED///+C78H/AAAAsXRSTlMAAAAAAAAAAAAAAAAAACtyOQEAAAAAAAAAF3fe/+qLIgAAAAAAAApXyP7/////12sSAAAAAoj5/////////qsOAAAIvf///////+EeAAAIu///3x4AAAi7///fHgAACLv//98eAAAIvf//////4R4AAAOU/v////////+3EAAAABBq1v//////434aAAAAAAAAIYrp//GdLwAAAAAAAAABOYZIBQAAAAAAAAAAAAAAAACOt1tJAAAAAWJLR0TPg97CaQAAAAlwSFlzAAALEgAACxIB0t1+/AAAAAd0SU1FB+MKDw06CRp6PvUAAAD2SURBVAjXY2BgYGBkYmZhZWPn4GQAAy5uHl4+fgFBIWERIE9UTFxCUkpaRlZOXkFRSZlBRVVNXUNTS1tHV0/fwNCIwdjE1MzcwnKjlbWNrZ29A4Ojk7OLq9umzVvcPTy9vH0YfP38A7Zu275j567dgUHBIQyhYeERe/bu23/g4KHIqOgYhti4+ITDR44eO37iZGJScgpDalp6RmbWqdNnzmbn5OblMxQUFhWXlJadK6+orKquqWWoq29obGpuaW1r7+js6u5h6O3rnzBx0uQpU6dNnzFz1mygi+fMnTd/wcJFi5csXbYc7KEVK1etXrN23foNQDYAUNJTSiksT4cAAAAldEVYdGRhdGU6Y3JlYXRlADIwMTktMTAtMTVUMTM6NTc6NDMrMDA6MDBKZh74AAAAJXRFWHRkYXRlOm1vZGlmeQAyMDE5LTEwLTE1VDEzOjU3OjQzKzAwOjAwOzumRAAAAABJRU5ErkJggg=="
          else if(res[i].site == 3) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAPCAMAAADqIa48AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JQAAgIMAAPn/AACA6QAAdTAAAOpgAAA6mAAAF2+SX8VGAAABuVBMVEUAAAA3sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk33Zwc2s042s040s04zs04zs040s041s041s04wtVD3ZwfRdhWSjiyugyLSdhXYcxK3gB6ciijNdxb3Zwf3Zwf3Zwf3Zwf5Zgb4Zgf4Zwf5Zgb4Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf///9TLSPdAAAAknRSTlMAAAAEUmYPAAAAAC3m+1wAAAAAGpn9/7szAAAAAD3d19LmbAAAAAAKXouOcRoAAAAOje3///awJAAAff///vv//60OItr/5mtWy//1UETy/5MAAFz9/3897/+qCQB3//52Fcf/+K6d6//pOgBa9f//////igMABHf0///+oRIAAAADWNHieg4AAAAAACEtAAAAAGBlrhUAAAABYktHRJKWBO8gAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAB3RJTUUH4woPDToxMniGawAAAAFvck5UBlHG4jkAAACtSURBVAjXY2BgZGJmYWVj52BgYODk4ubh5eMXEGRgEBIWERUTl5CUYpCWkZWTV1BUUlZhUFVT19DU0tbR1WPQNzA0MjYxNTO3YLC0sraxtbN3cHRicHZxdXP38PTy9mHw9fMPCAwKDgkNYwiPiIyKjomNi09gSExKTklNS8/IzGLIzsnNyy8oLCouYSgtK6+orKquqa1jqG9obGpuaW1r72Bg6Ozq7unt658wEQDIwilal7IBjQAAAERlWElmTU0AKgAAAAgAAgESAAMAAAABAAYAAIdpAAQAAAABAAAAJgAAAAAAAqACAAQAAAABAAAAWKADAAQAAAABAAAAhQAAAABG/6etAAAAKHRFWHRDb21tZW50AENyb3BwZWQgd2l0aCBlemdpZi5jb20gR0lGIG1ha2VyWZBFzQAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAxOS0xMC0xNVQxMzo1ODo0MSswMDowMCzyVFwAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMTktMTAtMTVUMTM6NTg6NDErMDA6MDBdr+zgAAAAEnRFWHRleGlmOkV4aWZPZmZzZXQAMzituL4jAAAAF3RFWHRleGlmOlBpeGVsWERpbWVuc2lvbgA4OIuNV8IAAAAYdEVYdGV4aWY6UGl4ZWxZRGltZW5zaW9uADEzM2kZo2IAAAASdEVYdFNvZnR3YXJlAGV6Z2lmLmNvbaDDs1gAAAAASUVORK5CYII="
          this.contents.push(res[i]);
          this.active[i] = false;
        }
      });
    },
    pageload: function(callback) {
      var chk,i;
      var data = {};
      data.company = [];
      data.cpu = [];
      data.ram = [this.ramSlider.value[0],this.ramSlider.value[1]];
      data.display = [this.displaySlider.value[0],this.displaySlider.value[1]];
      data.ssd = [this.ssdSlider.value[0],this.ssdSlider.value[1]];

      chk = document.getElementsByName('company')
      for( i = 0 ; i < chk.length ; i++){
        if(chk[i].checked === true) data.company.push(chk[i].id)
      }
      chk = document.getElementsByName('cpu')
      for( i = 0 ; i < chk.length ; i++){
        if(chk[i].checked === true) data.cpu.push(chk[i].id)
      }
      var chkRam = document.getElementById('ignoreRam')
      var chkDisplay = document.getElementById('ignoreDisplay')
      var chkSSD = document.getElementById('ignoreSSD')
      var chkPrice = document.getElementById('ignorePrice')

      data.isIgnoreRam = !chkRam.checked
      data.isIgnoreSSD = !chkSSD.checked
      data.isIgnoreDisplay = !chkDisplay.checked
      data.isIgnorePrice = !chkPrice.checked
      console.log(data)
      $.ajax({
        type: "GET",
        data: data,
        contentType: "application/json",
        url: "http://52.78.210.26:8080/laptop/:"+ this.page,
        success: res => {
          console.log("/laptop..");
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
    pageload_: function(pagen_) {
      this.page = pagen_
      this.check(res => {
        this.contents = [];
        for (var i = 0; i < res.length; i++) {
          if (res[i].price.length === 1) res[i].price = "0원";
          res[i].time = res[i].time.substring(0, 10);
          res[i].price = new Intl.NumberFormat('ko-KR', { style: 'currency', currency: 'KRW' }).format(res[i].price)
          if(res[i].site == 1) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAPCAMAAAAMCGV4AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAACW1BMVEUAAADsZSzmYivwZy3yZy3yZy3yZy3yZy3yZy3xZyzuZSzuZSzzaC3vZizzaC30aC30aC30aC30aC30aC3zaC3jYyvvZizlYirvZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizuZizvZizuZizuZizsZSzsZSzsZSzsZSzsZSzsZSzsZSzsZSzsZSztZizuZizvZiztZSzuZizzaC30aC3wZyzwZyy7UiPsZSzsZSz4ai7xZy0AAADgYSztZizwZy3tZSzvZizzaC3vZizuZizuZizvZizvZizvZizvZizuZizuZizuZiztZSzwZy3tZSzuZizxZy3vZizvZizvZizvZizvZS7wZiztZSzsZSztZSzvZiztZSzwZy3tZSzvZizyZy3uZizuZizuZizvZiztZizvZizuZizuZiztZSzvZiztZSzwZy3tZSzuZiz0aC3wZizvZizvZizxZy3wZy3vZizwZy3wZizyZy3vZi3tZizwZy3uZizuZizuZizuZizuZizuZizuZizxZy3uZizuZizuZizuZizuZizuZizvZiz2aS7vZizuZizuZizuZizuZizuZizuZiztZSzuZizuZizuZizuZizuZizvZi3xZy3/cTHxZy3zaC3zaC3zaC3vZi3tZiztZSzxZy3zaC3zaC3xZy3xZy3wZi3wZy3vZizxZy3zaC30aC30aC3vZi3vZizvZizxZy3yaC30aC3yZy3vZizwZi3uZizxZyztZizyZy3wZy3wZyzpXCnvZizwZizxZy3wZy3wZyzsZSz///9QlC4tAAAAyHRSTlMAAAAAAAAAAAAAAAAAAAcNDQ0NDQoAAABFs8TExMTExMTEwHQLH8qVWllaXF1dXFpbcdhfPdQjBkAzAAAAAikAAKyPPtEpiLi/XwADLrQnBamQPtBAvidYqAE9vOu4OKeQPtEunqGzdQAMRcM+DamQPtIgFmdWCQAABkUDAKqRJ9NzMzM0LgkkNzQ1StFuAmPO09PT25zK3NLT1JYVAAAYJCQiR9X7YCAkHwYAAAAAAAAAADq3MwAAAAAAAAAADgQAAAAAAAAAAGt/iEYAAAABYktHRMgdulfKAAAACXBIWXMAAAsSAAALEgHS3X78AAAAB3RJTUUH4woPDTsREA2X4gAAAO1JREFUCNdjYGRiZmFlAwF2Dk4ubgYeXj5+AUEQEBIWERVjEJeQlJKWkZWTk1dQVFJWYVBVU9fQ1NLW0dXTNzA0MmYwMTUzt7C0sraxtbN3cHRicHZxdXP38PTy9vH18w8IZAgKDgkNC4+IjIqOiY2LT2BITEpOSU1Lz8jMys7JzctnKCgsKi4pLSuvqKyqrqmtY6hvaGxqbmlta+/o7Oru6WXo658wcdLkKVOnTZ8xc9bsOQxz581fsHDR4iVLly1fsXLVaoY1a9et37Bx0+YtW7dt37FzFwMY7N6zd9/+AwcZ4ODQ4SNHjx0HMgAaXU43wzbSkQAAACh0RVh0Q29tbWVudABDcm9wcGVkIHdpdGggZXpnaWYuY29tIEdJRiBtYWtlclmQRc0AAAAldEVYdGRhdGU6Y3JlYXRlADIwMTktMTAtMTVUMTM6NTk6MTMrMDA6MDAcTyAvAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDE5LTEwLTE1VDEzOjU5OjEzKzAwOjAwbRKYkwAAABJ0RVh0U29mdHdhcmUAZXpnaWYuY29toMOzWAAAAABJRU5ErkJggg=="
          else if(res[i].site == 2) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAPCAMAAAAMCGV4AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAACcFBMVEUAAACGAAD1AAD0AAD4AAD4AAD4AAD2AAD4AACUAAD0AAD/AAD4AAD6AAD4AAD4AAD3AADxAAD4AADsAADyAADmAAD4AAD3AAD4AAD3AAD3AAD3AAD3AAD3AAD3AAD4AAD6AAD3AAD7AADyAAD/AAD5AAD2AAD3AAD3AAD3AAD3AAD3AAD3AAD3AAD3AAD4AAD3AAD7AAD/AAD5AAD9AAD4AAD3AAD3AAD3AAD5ODj6cnL4ICD3AAD3AAD4AAD5AAD5AAD9AAD/AAD4AAD3AAD3AAD3AgL4ISH3AAD3AAD4AAD7AAD7AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD4AAD8AAD8AAD9AAD/AAD4AAD3AAD3AAD3AAD3AAD3AAD4AAD7AAD8AAD5AAD8AAD4AAD3AAD3AAD3AQH6bm73AAD3AAD3AAD3AAD4AAD6AAD6AAD/AAD6AAD2AAD4AAD3AAD3AQH3EBD3AAD3AAD3AAD3AAD3AAD4AAD6AADaAADZAAD6AAD3AAD4AAD3AAD3AAD3AAD3AAD3AAD4AAD4AAD/AAD4AAD7AAD1AAD3AADxAAD4AAD0AAD4AAD3AAD3AAD3AAD4AAD0AAD1AAAIAAC7AAD3AAD2AAD4AAD/AAD5AAD3AAD4AADsAAD7d3f8pKT////+3d33AAD4GRn+1tb////8nJz3FBT3AgL3AAD5Rkb/+fn////+6Oj7hYX3BQX3AAD5ODj8oaH/+Pj9xcX4JSX3AAD5U1P9y8v4Li73AAD5QED///+C78H/AAAAsXRSTlMAAAAAAAAAAAAAAAAAACtyOQEAAAAAAAAAF3fe/+qLIgAAAAAAAApXyP7/////12sSAAAAAoj5/////////qsOAAAIvf///////+EeAAAIu///3x4AAAi7///fHgAACLv//98eAAAIvf//////4R4AAAOU/v////////+3EAAAABBq1v//////434aAAAAAAAAIYrp//GdLwAAAAAAAAABOYZIBQAAAAAAAAAAAAAAAACOt1tJAAAAAWJLR0TPg97CaQAAAAlwSFlzAAALEgAACxIB0t1+/AAAAAd0SU1FB+MKDw06CRp6PvUAAAD2SURBVAjXY2BgYGBkYmZhZWPn4GQAAy5uHl4+fgFBIWERIE9UTFxCUkpaRlZOXkFRSZlBRVVNXUNTS1tHV0/fwNCIwdjE1MzcwnKjlbWNrZ29A4Ojk7OLq9umzVvcPTy9vH0YfP38A7Zu275j567dgUHBIQyhYeERe/bu23/g4KHIqOgYhti4+ITDR44eO37iZGJScgpDalp6RmbWqdNnzmbn5OblMxQUFhWXlJadK6+orKquqWWoq29obGpuaW1r7+js6u5h6O3rnzBx0uQpU6dNnzFz1mygi+fMnTd/wcJFi5csXbYc7KEVK1etXrN23foNQDYAUNJTSiksT4cAAAAldEVYdGRhdGU6Y3JlYXRlADIwMTktMTAtMTVUMTM6NTc6NDMrMDA6MDBKZh74AAAAJXRFWHRkYXRlOm1vZGlmeQAyMDE5LTEwLTE1VDEzOjU3OjQzKzAwOjAwOzumRAAAAABJRU5ErkJggg=="
          else if(readys[i].site == 3) res[i].site = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAPCAMAAADqIa48AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JQAAgIMAAPn/AACA6QAAdTAAAOpgAAA6mAAAF2+SX8VGAAABuVBMVEUAAAA3sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk03sk33Zwc2s042s040s04zs04zs040s041s041s04wtVD3ZwfRdhWSjiyugyLSdhXYcxK3gB6ciijNdxb3Zwf3Zwf3Zwf3Zwf5Zgb4Zgf4Zwf5Zgb4Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf3Zwf///9TLSPdAAAAknRSTlMAAAAEUmYPAAAAAC3m+1wAAAAAGpn9/7szAAAAAD3d19LmbAAAAAAKXouOcRoAAAAOje3///awJAAAff///vv//60OItr/5mtWy//1UETy/5MAAFz9/3897/+qCQB3//52Fcf/+K6d6//pOgBa9f//////igMABHf0///+oRIAAAADWNHieg4AAAAAACEtAAAAAGBlrhUAAAABYktHRJKWBO8gAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAB3RJTUUH4woPDToxMniGawAAAAFvck5UBlHG4jkAAACtSURBVAjXY2BgZGJmYWVj52BgYODk4ubh5eMXEGRgEBIWERUTl5CUYpCWkZWTV1BUUlZhUFVT19DU0tbR1WPQNzA0MjYxNTO3YLC0sraxtbN3cHRicHZxdXP38PTy9mHw9fMPCAwKDgkNYwiPiIyKjomNi09gSExKTklNS8/IzGLIzsnNyy8oLCouYSgtK6+orKquqa1jqG9obGpuaW1r72Bg6Ozq7unt658wEQDIwilal7IBjQAAAERlWElmTU0AKgAAAAgAAgESAAMAAAABAAYAAIdpAAQAAAABAAAAJgAAAAAAAqACAAQAAAABAAAAWKADAAQAAAABAAAAhQAAAABG/6etAAAAKHRFWHRDb21tZW50AENyb3BwZWQgd2l0aCBlemdpZi5jb20gR0lGIG1ha2VyWZBFzQAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAxOS0xMC0xNVQxMzo1ODo0MSswMDowMCzyVFwAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMTktMTAtMTVUMTM6NTg6NDErMDA6MDBdr+zgAAAAEnRFWHRleGlmOkV4aWZPZmZzZXQAMzituL4jAAAAF3RFWHRleGlmOlBpeGVsWERpbWVuc2lvbgA4OIuNV8IAAAAYdEVYdGV4aWY6UGl4ZWxZRGltZW5zaW9uADEzM2kZo2IAAAASdEVYdFNvZnR3YXJlAGV6Z2lmLmNvbaDDs1gAAAAASUVORK5CYII="
          this.contents.push(res[i]);
          this.active[i] = false;
        }
      });
    },
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
                <a href="javascript: void(0);">전자기기</a>
              </li>
              <li class="breadcrumb-item active">노트북</li>
            </ol>
          </div>
          <h4 class="page-title">Laptop</h4>
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
              <input id="삼성" type="checkbox" name="company" @click="load"/>
              <label for="company_checkbox0">삼성</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="LG" type="checkbox" name="company" @click="load" />
              <label for="company_checkbox1">LG</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="APPLE" type="checkbox" name="company" @click="load" />
              <label for="company_checkbox2">Apple</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="HP" type="checkbox" name="company" @click="load" />
              <label for="company_checkbox3">HP</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="ASUS" type="checkbox" name="company" @click="load" />
              <label for="company_checkbox4">ASUS</label>
            </div>
          </div>

          <div class="row">
            <label class="col-sm-3 col-form-label">CPU</label>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="Intel i3" type="checkbox" name="cpu" @click="load" />
              <label for="cpu_checkbox0">I3</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="Intel i5" type="checkbox" name="cpu" @click="load" />
              <label for="cpu_checkbox1">I5</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="Intel i7" type="checkbox" name="cpu" @click="load" />
              <label for="cpu_checkbox2">I7</label>
            </div>
            <div class="col-sm-1 checkbox checkbox-primary form-check-inline">
              <input id="Intel i9" type="checkbox" name="cpu" @click="load" />
              <label for="cpu_checkbox3">I9</label>
            </div>
          </div>

          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label">RAM</label>
            <div class="ignoreCheck">
              <input id="ignoreRam" type="checkbox" name="ignore" @click="load"></input>
            </div>
              <div class="col-sm-1 ">
                <input class="form-control form-control-sm" v-model="ramSlider.value[0]" @input="clearErrorMsgRam" />
              </div>
              <div class="col-sm-6">
              <vue-slider
                v-model="ramSlider.value"
                :min="ramSlider.min"
                :max="ramSlider.max"
                :tooltip="ramSlider.errorMsg ? 'none' : 'always'"
                :marks="[ramSlider.min, ramSlider.max]"
                @error="error"
                @change="clearErrorMsgRam"
                @drag-end="load"
                @drag-start="ignoreRamFalse"
              ></vue-slider>
              </div>
              <div class="col-sm-1">
                <input class="form-control form-control-sm" v-model="ramSlider.value[1]" @input="clearErrorMsgRam" />
              </div>
            </div>

          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label">SSD / HDD</label>
            <div class="ignoreCheck">
              <input id="ignoreSSD" type="checkbox" name="ignore" @click="load"></input>
            </div>
            <div class="col-sm-1 ">
              <input class="form-control form-control-sm" v-model="ssdSlider.value[0]" @input="clearErrorMsgSSD" />
            </div>
            <div class="col-sm-6">
            <vue-slider
              v-model="ssdSlider.value"
              :min="ssdSlider.min"
              :max="ssdSlider.max"
              :tooltip="ssdSlider.errorMsg ? 'none' : 'always'"
              :marks="[ssdSlider.min, ssdSlider.max]"
              @error="error"
              @change="clearErrorMsgSSD"
              @drag-end="load"
              @drag-start="ignoreSSDFalse"
            ></vue-slider>
            </div>
            <div class="col-sm-1">
              <input class="form-control form-control-sm" v-model="ssdSlider.value[1]" @input="clearErrorMsgSSD" />
            </div>
          </div>

          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label">모니터 크기</label>
            <div class="ignoreCheck">
              <input id="ignoreDisplay" type="checkbox" name="ignore" @click="load"></input>
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
              @drag-start="ignoreDisplayFalse"
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
            <div class="ignoreCheck">
              <input id="ignorePrice" type="checkbox" name="ignore" @click="load"></input>
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
              @change="clearErrorMsgDisplay"
              @drag-end="load"
              @drag-start="ignorePriceFalse"
            ></vue-slider>
            </div>
            <div class="col-sm-1">
              <input class="form-control form-control-sm" v-model="priceSlider.value[1]" @input="clearErrorMsgPrice" />
            </div>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label"></label>
          </div>
          <div class="row">
            <label class="col-sm-3 col-form-label">제목 검색</label>
            <div class="app-search-box">
              <div class="input-group">
                <input
                  type="text"
                  class="form-control search-box"
                  id="search-box"
                  placeholder="Search..."
                />
                <div class="input-group-append">
                  <button
                    class="btn btn-box"
                    @click="load"
                  >
                    <i class="fe-search search-icon"></i>
                  </button>
                </div>
              </div>
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
                        <th id="tech-companies-1-col-0" class="table_site" data-priority="1">사이트</th>
                        <th class="overviewhead"></th>
                        <th id="tech-companies-1-col-1" class="table_title" data-priority="1">글 제목</th>
                        <th id="tech-companies-1-col-2" class="table_price" data-priority="1">가격</th>
                        <th id="tech-companies-1-col-3" class="table_time" data-priority="1">날짜</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="(con) in contents" :key="con._id">
                        <th data-org-colspan="1" data-columns="tech-companies-1-col-0">
                          <a
                            :href="con.url"
                          ><img 
                          :src="con.site"></a>
                        </th>
                        <!-- <button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="bottom" title="" data-original-title="Tooltip on bottom">Tooltip on bottom</button> -->
                        <!-- <th class="submenu" data-org-colspan="1" data-columns="tech-companies-1-col-0">{{con.title}}content..~~~</th> -->
                        <td class="overview">
                          
                        </td>
                        
                        <td
                          data-org-colspan="1"
                          data-priority="1"
                          data-columns="tech-companies-1-col-1"
                          data-toggle="popover"
                          data-placement="top"
                          :title="con.text"
                          :data-content="con.text"
                          data-original-title=""
                          :aria-describedby="con._id"
                        >{{con.title}}
                        <div class="overview">
                          <object 
                            type="text/html"
                            width="800px"
                            height="1000px"
                            style="overflow:auto;border:1px solid rgb(200,200,200,0.5);"
                            :data="con.url"
                          >
                          </object>
                        </div>
                        </td>
                        <!-- <td
                          data-org-colspan="1"
                          data-priority="1"
                          data-columns="tech-companies-1-col-1"
                        >
                          <a 
                          href="#" 
                          title="Header" 
                          data-toggle="popover" 
                          data-placement="bottom" 
                          data-content="con.text">
                            {{con.title}}
                          </a>
                        </td>  -->

                        <!-- <button 
                          type="button" 
                          class="btn btn-secondary" 
                          data-container="body" 
                          title="" 
                          data-toggle="popover" 
                          data-placement="right" 
                          data-content="Vivamus sagittis lacus vel augue laoreet rutrum faucibus." 
                          data-original-title="" 
                          aria-describedby="popover936424"
                        >Popover on right</button> -->
                        <!-- <button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="top" title="" data-original-title="Tooltip on top">Tooltip on top</button> -->
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
          <!-- page num -->
          <!-- <div>
            <nav>
                <ul class="pagination pagination-split">
                    <li class="page-item">
                        <a class="page-link" href="#" aria-label="Previous">
                            <span aria-hidden="true">«</span>
                            <span class="sr-only">Previous</span>
                        </a>
                    </li>
                    <li v-for="i in this.pagerange" v-if="i > 0" class="page-item" @click="pageload_(i)"><a class="page-link" href="#">{{i}}</a></li>
                    <!-- <li class="page-item active"><a class="page-link" href="#">1</a></li>
                    <li class="page-item"><a class="page-link" href="#">2</a></li>
                    <li class="page-item"><a class="page-link" href="#">3</a></li>
                    <li class="page-item"><a class="page-link" href="#">4</a></li>
                    <li class="page-item"><a class="page-link" href="#">5</a></li> --
                    <li class="page-item">
                        <a class="page-link" href="#" aria-label="Next">
                            <span aria-hidden="true">»</span>
                            <span class="sr-only">Next</span>
                        </a>
                    </li>
                </ul>
            </nav>
          </div> -->
        </div>
      </div>
    </div>
  </Layout>
</template>


<style>
  .table_title{
    width:656px;
  }
  .table_price{
    width:160px;
  }
  .table_time{
    width:160px;
  }
  .table_site{
    width:80px;
  }

  .ignoreCheck{
    width:20px;
  }

  .search-box{
    background-color: rgba(50,50,50,0.01);
    border-color: rgba(100,100,100,0.07) !important;
    border-radius: 30px 0px 0px 30px;
    width:400px !important;
  }

  .btn-box{
    background-color: rgba(50,50,50,0.01);
    border-color: rgba(100,100,100,0.07) !important;
    border-radius: 0px 30px 30px 0px;
  }

  .search-icon{
    color: rgba(150,150,150,1)
  }

  .overviewhead{
    width:0px;
    padding: 0 0 0 0 !important;
  }

  .overview{
    position:relative;
    top:10px;
    padding: 0 0 0 0 !important;
    /* visibility:hidden; */
    overflow:visible;
  }

  .overview object{
    border: solid 1px black;
    display:none;
  }


  td:hover object{
    position:absolute;
    display:block;
    z-index:100;
  }

  .responsive-table-plugin{
    overflow:visible;
  }

  .card-box{
    overflow:visible;
  }
/* 
  .overview:hover object{
    display:block;
  } */
</style>
