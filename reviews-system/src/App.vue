<template>
  <div id="app">
    <v-app style="width: 100%; height: 100%">
      <login v-if="!_staff.name"></login>
      <div v-else>
        <div class="navi">
          <v-card class="card">
            <v-navigation-drawer permanent>
              <v-list-item>
                {{ title }}
              </v-list-item>
              <v-list-item class="caption">
                {{ `you are in ${this._staff.type} page!` }}
              </v-list-item>
              <v-list dense nav>
                <v-list-item
                  v-for="item in sideNav"
                  :key="item.title"
                  link
                  @click="selectedItem = item.title"
                >
                  <font-awesome-icon
                    style="margin-right: 5px"
                    :icon="item.icon"
                  />
                  {{ item.title }}
                </v-list-item>
              </v-list>
            </v-navigation-drawer>
          </v-card>
        </div>
        <v-container class="container">
          <add-staff-info
            :allStaffs="_allStaffs"
            :showLog="logStatus"
            @showLog="statusChange"
          ></add-staff-info>
          <v-toolbar class="toolbar" dense flat>
            <v-btn
              v-if="this._staff.type !== 'employee'"
              outlined
              color="green"
            >
              save changes
              <font-awesome-icon class="icon" :icon="['fas', 'check-circle']" />
            </v-btn>
            <v-btn
              v-if="this._staff.type !== 'employee'"
              outlined
              @click="logStatus = true"
            >
              add
              <font-awesome-icon class="icon" :icon="['fas', 'user-plus']" />
            </v-btn>
            <v-btn outlined @click="langSwitch"
              >lan
              <font-awesome-icon
                class="icon"
                :icon="['fas', 'globe-americas']"
              />
            </v-btn>
            <v-btn outlined @click="logout"
              >logout
              <font-awesome-icon class="icon" :icon="['fas', 'sign-out-alt']" />
            </v-btn>
          </v-toolbar>
          <router-view :selectedItem="selectedItem"></router-view>
        </v-container>
      </div>
    </v-app>
  </div>
</template>

<script lang="ts">
import {Component, Watch, Vue} from 'vue-property-decorator';
import {State, Action, Getter, namespace} from 'vuex-class';
import {FontAwesomeIcon} from '@fortawesome/vue-fontawesome';
import {library} from '@fortawesome/fontawesome-svg-core';
import {faUser, faUserPlus, faSignOutAlt, faAddressBook, faGlobeAmericas, faCheckCircle} from '@fortawesome/free-solid-svg-icons';

import AddStaffInfo from '@/components/AddStaffInfo.vue';
import Login from './Login.vue';
import * as types from './typedef';

library.add(faUserPlus, faSignOutAlt, faAddressBook, faGlobeAmericas, faCheckCircle, faUser);

Vue.component('font-awesome-icon', FontAwesomeIcon);
Vue.component('add-staff-info', AddStaffInfo);
Vue.component('login', Login);

@Component
export default class App extends Vue {
  get title() {
    return `${this.$i18n.t('hello')}, ${this._staff.name}`;
  }
  @Action('initAPI') private _inintAPI!: () => Promise<void>;
  @Action('getReviewByName') private _getReviewByName!: (name: string) => Promise<void>;
  @Action('getAllStaff') private _getAllStaff!: () => Promise<void>;
  @Action('logout') private _logout!: () => Promise<void>;
  @Action('setLang') private _setLang!: (lang: string) => Promise<void>;
  @Getter('staff') private _staff!: types.StaffResponse;
  @Getter('allStaffs') private _allStaffs!: types.StaffResponse[];
  @Getter('reviews') private _reviews!: types.ReviewResponse;
  private selectedItem: string = '';
  private sideNav: types.SideNav[] = [];
  private type: string = 'employee';
  private logStatus: boolean = false;

  private right = null;

  private async logout() {
    this.$router.push('/');
    this.selectedItem = '';
    await this._logout();
  }

  @Watch('_staff') private async onChanged() {
    if (this._staff.type === 'employee') {
      this.$router.push({name: 'EmployeeHome'});
      this.sideNav = [{title: 'myself', icon: ['fas', 'user']}];
      await this._getReviewByName(this._staff.name);
      this._reviews.toReview.forEach((title: string) => this.sideNav.push({title, icon: ['fas', 'user']}));
    } else {
      await this._getAllStaff();
      this.$router.push({name: 'AdminHome'});
      this.sideNav = [];
      const teams: string[] = [];
      this._allStaffs.forEach((staff: types.StaffResponse) => {
        if (!teams.includes(staff.team)) teams.push(staff.team);
      });
      teams.forEach((team: string) => this.sideNav.push({title: `${team} Team`, icon: ['fas', 'address-book']}));
    }
  }

  private async mounted() {
    await this._inintAPI();
  }

  private statusChange(status: boolean) {
    this.logStatus = status;
  }

  private langSwitch() {
    const currentLang: string = localStorage.getItem('lang')!;
    if (currentLang === 'en') this._setLang('ja');
    else this._setLang('en');
  }

  private async created() {
    const currentLang: string = localStorage.getItem('lang')!;
    if (!currentLang) {
      await this._setLang('en');
      location.reload();
    }
  }

}

/**
 * It's used to block mouse right click menu
 */
document.oncontextmenu = (e: any) => {
  if (window.event) {
    e = window.event;
    e.returnValue = false;
  } else {
    e.preventDefault();
  }
};
</script>

<style lang="scss" scoped>
.navi {
  display: flex;
  width: 256px;
  height: 100% !important;
  position: fixed;
}

.card {
  height: 100%;
  width: 256px;
}

.container {
  margin-left: 256px;
  height: 100%;
  width: 100%;
  padding: 0px !important;
}

.toolbar {
  width: 100%;
  height: auto;
  display: flex;
  justify-content: flex-end;
}

.main {
  height: 100px;
}

.icon {
  margin-left: 5px;
}
</style>
