<template>
  <v-app>
     <v-navigation-drawer
      v-model="drawer"
      app
      clipped
    >
    <v-list>
      <v-list-group
        v-for="(abbrevs, abbrevI) in Bible"
        :key="abbrevI"
        color="red"
        append-icon="mdi-chevron-down">
        <template v-slot:activator>
          <v-list-item-content>
            <v-list-item-title v-text="abbrevs.abbrev"></v-list-item-title>
          </v-list-item-content>
        </template>

        <v-row justify="start" no-gutters>
          <v-col cols="3"
          v-for="(chapter, chapterI) in abbrevs.chapters"
          :key="chapterI"
          >
            <v-btn text block @click="selectChapter(abbrevI, chapterI)">{{ (chapterI+1) }}</v-btn>
          </v-col>
        </v-row>
      </v-list-group>
    </v-list>
    </v-navigation-drawer>
    
    <v-app-bar
      app
      clipped-left
      color="grey lighten-4"
      dense
    >
    <v-btn class="mx-1" icon @click.stop="drawer = !drawer">
      <v-icon>mdi-menu</v-icon>
    </v-btn>
      
      <v-toolbar-title>
        <span>{{ selected ? selectedAbbrev + ' ' + selectedChapter +'장' : ''}}</span>
      </v-toolbar-title>
      <v-spacer/>
    </v-app-bar>

    <v-content>
      <v-list dense flat v-if="selected">
      <v-list-item
        v-for="(item,i) in bibleContent"
        :key="i">
        <span >{{ (i) }} &nbsp;&nbsp; {{ item }}</span>
      </v-list-item>
      </v-list>
      <v-card>
      </v-card>
      <v-row justify="center" align="center" v-if="!selected">
        <v-col xs="12" sm="12" md="12" lg="8" xl="6">
      <v-hover v-slot:default="{ hover }" class="ma-5" >
          <v-card :elevation="hover ? 12 : 2">
            <v-list>
              <v-list-item>
                  <v-card-text class="text-left title font-weight-bold">성경 이용 방법 안내</v-card-text>
              </v-list-item>
              <v-divider class="mx-2"/>
              <v-list-item>
                <v-list-item-avatar>
                  <span>1</span>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title>상단 메뉴 클릭</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-avatar>
                  <span>2</span>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title>성경 클릭</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-avatar>
                  <span>3</span>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title>장 클릭</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-card>
        </v-hover>
        </v-col>
      </v-row>
    </v-content>
    <v-footer color="red accent-1" app inset padless>
    <v-spacer></v-spacer>
    <div class="white--text caption">
      <span class="mr-2">&copy; 2020. created by seonggn-yun.</span></div>
  </v-footer>
  </v-app>
</template>

<script>
import Bible from '@/assets/ko_rev.json'
export default {
  name: "App",
  data() {
    return {
      drawer: null,
      Bible: Bible,
      oldTestament: [],
      newTestament: [],
      bibleContent: [],
      selected: false,
      selectedAbbrev: '',
      selectedChapter: ''
    }
  },
  methods: {
    selectChapter(abbrevI, chapterI) {
      this.bibleContent = Bible[abbrevI].chapters[chapterI];
      localStorage.abbrev = abbrevI;
      localStorage.chapter = chapterI;
      this.selectedAbbrev = Bible[localStorage.abbrev].abbrev;
      this.selectedChapter = localStorage.chapter * 1 + 1;
      this.selected = true;
      this.drawer = false;
    }
  },
  created() {
    if(localStorage.abbrev && localStorage.chapter) {
      this.bibleContent = Bible[localStorage.abbrev].chapters[localStorage.chapter];
      this.selectedAbbrev = Bible[localStorage.abbrev].abbrev;
      this.selectedChapter = localStorage.chapter * 1 + 1;
      this.selected = true;
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Noto+Sans+KR:400,500,700&display=swap&subset=korean');
#app {
  font-family: 'Noto Sans KR', sans-serif;
}
</style>