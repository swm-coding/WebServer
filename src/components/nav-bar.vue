<script>
import { authComputed } from '@state/helpers'

export default {
  components: {},
  props: {
    user: {
      type: Object,
      required: false,
      default: () => ({}),
    },
    isMenuOpened: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  data() {
    return {}
  },
  computed: {
    ...authComputed,
  },
  methods: {
    onMenuClick(event) {
      event.preventDefault()
      const nextEl = event.target.nextSibling
      if (nextEl && !nextEl.classList.contains('open')) {
        const parentEl = event.target.parentNode
        if (parentEl) {
          parentEl.classList.remove('open')
        }

        nextEl.classList.add('open')
      } else if (nextEl) {
        nextEl.classList.remove('open')
      }
      return false
    },
  },
}
</script>

<template>
  <!-- Topbar Start -->
  <div class="topbar-menu">
    <div class="container-fluid">
      <b-collapse id="navigation" v-model="isMenuOpened">
        <!-- Navigation Menu-->
        <ul class="navigation-menu">
          <li class="has-submenu active"><router-link tag="a" to="/"><i class="mdi mdi-view-dashboard"></i>Home</router-link></li>
          <li class="has-submenu">
            <a href="#" @click="onMenuClick"><i class="mdi mdi-share-variant"></i>컴퓨터 - 노트북<div class="arrow-down"></div></a>
            <ul class="submenu megamenu">
              <li><ul>
              <li><router-link tag="a" to="/l">데스크탑</router-link></li>
              <li><router-link tag="a" to="/l">모니터</router-link></li>
              <li><router-link tag="a" to="/l">키보드</router-link></li>
              <li><router-link tag="a" to="/l">마우스</router-link></li>
              <li><router-link tag="a" to="/l">공유기</router-link></li>
              </ul></li>
              <li><ul>
              <li><router-link tag="a" to="/laptop">노트북</router-link></li>
              <li><router-link tag="a" to="/l">복합기</router-link></li>
              <li><router-link tag="a" to="/l">프린터</router-link></li>
              <li><router-link tag="a" to="/l">스캐너</router-link></li>
              </ul></li>
              <li><ul>
              <li><router-link tag="a" to="/l">스피커</router-link></li>
              <li><router-link tag="a" to="/l">헤드셋</router-link></li>
              <li><router-link tag="a" to="/l">USB</router-link></li>
              </ul></li>
            </ul>      
          </li>
          
          <li class="has-submenu">
            <a href="#" @click="onMenuClick"><i class="mdi mdi-share-variant"></i>모바일 - 카메라 - 디지털<div class="arrow-down"></div></a>
            <ul class="submenu megamenu">
              <li><ul>
              <li><router-link tag="a" to="/smartphone">스마트폰</router-link></li>
              <li><router-link tag="a" to="/l">태블릿</router-link></li>
              <li><router-link tag="a" to="/l">스마트워치</router-link></li>
              </ul></li>
              <li><ul>
              <li><router-link tag="a" to="/l">디지털카메라</router-link></li>
              <li><router-link tag="a" to="/l">DSLR</router-link></li>
              <li><router-link tag="a" to="/l">액션캠</router-link></li>
              <li><router-link tag="a" to="/l">카메라렌즈</router-link></li>
              </ul></li>
              <li><ul>
              <li><router-link tag="a" to="/l">PlayStation</router-link></li>
              <li><router-link tag="a" to="/l">XBox</router-link></li>
              <li><router-link tag="a" to="/l">닌텐도</router-link></li>
              <li><router-link tag="a" to="/l">스피커</router-link></li>
              <li><router-link tag="a" to="/l">이어폰</router-link></li>
              </ul></li>
            </ul>      
          </li>

          <li class="has-submenu">
            <a href="#" @click="onMenuClick"><i class="mdi mdi-share-variant"></i>가전제품<div class="arrow-down"></div></a>
            <ul class="submenu megamenu">
              <li><ul>
              <li><router-link tag="a" to="/refrigerator">냉장고</router-link></li>
              <li><router-link tag="a" to="/l">김치냉장고</router-link></li>
              <li><router-link tag="a" to="/l">전기밥솥</router-link></li>
              <li><router-link tag="a" to="/l">전자/전기레인지</router-link></li>
              <li><router-link tag="a" to="/l">오븐</router-link></li>
              <li><router-link tag="a" to="/l">전기포트</router-link></li>
              <li><router-link tag="a" to="/l">믹서기</router-link></li>
              <li><router-link tag="a" to="/l">커피머신</router-link></li>
              <li><router-link tag="a" to="/l">식기세척기</router-link></li>
              <li><router-link tag="a" to="/l">식기건조기</router-link></li>
              <li><router-link tag="a" to="/l">음식물처리기</router-link></li>
              </ul></li>
              <li><ul>
              <li><router-link tag="a" to="/washer">세탁기</router-link></li>
              <li><router-link tag="a" to="/l">청소기</router-link></li>
              <li><router-link tag="a" to="/l">다리미</router-link></li>
              <li><router-link tag="a" to="/l">의류건조기</router-link></li>
              <li><router-link tag="a" to="/l">헤어기기</router-link></li>
              </ul></li>
              <li><ul>
              <li><router-link tag="a" to="/tv">TV</router-link></li>
              <li><router-link tag="a" to="/l">에어컨</router-link></li>
              <li><router-link tag="a" to="/l">난방기</router-link></li>
              <li><router-link tag="a" to="/l">공기청정기</router-link></li>
              <li><router-link tag="a" to="/l">가습기</router-link></li>
              <li><router-link tag="a" to="/l">정수기</router-link></li>
              </ul></li>
            </ul>      
          </li>


        </ul>
        <!-- End navigation menu -->
        <div class="clearfix"></div>
      </b-collapse>
      <!-- end #navigation -->
    </div>
    <!-- end container -->
  </div>
  <!-- end navbar-custom -->
</template>

<style lang="scss">

</style>
