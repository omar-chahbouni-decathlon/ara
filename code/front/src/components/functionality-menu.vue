<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ~ Copyright (C) 2019 by the ARA Contributors
  ~
  ~ Licensed under the Apache License, Version 2.0 (the "License");
  ~ you may not use this file except in compliance with the License.
  ~ You may obtain a copy of the License at
  ~
  ~ 	 http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing, software
  ~ distributed under the License is distributed on an "AS IS" BASIS,
  ~ WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  ~ See the License for the specific language governing permissions and
  ~ limitations under the License.
  ~
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<template>
  <!--
    iView's Menu cannot Ctrl+click to open in a new tab,
    and we must compute active-name by ourself instead of relying on router-link putting the active-class on correct items.
    So we use Menu classes with router-links.
  -->
  <ul class="subMenu ivu-menu ivu-menu-light ivu-menu-horizontal">
    <router-link v-for="link in links" :key="link.name" :to="to(link)"
                 class="ivu-menu-item" active-class="ivu-menu-item-active ivu-menu-item-selected">
      {{link.name}}
    </router-link>
  </ul>
</template>

<script>
  export default {
    name: 'functionality-menu',

    computed: {
      links () {
        return [
          { name: 'COVERAGE', routeName: 'functionality-coverage' },
          { name: 'CARTOGRAPHY', routeName: 'functionality-cartography' },
          { name: 'ASSIGNATIONS', routeName: 'functionality-assignations' }
        ]
      }
    },

    methods: {
      to (link) {
        return {
          name: link.routeName,
          params: {
            projectCode: this.$route.params.projectCode
          }
        }
      }
    }
  }
</script>
