<template>
  <div>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app>
      <v-list dense>
        <template v-for="item in items">
          <v-layout v-if="item.heading" :key="item.heading" align-center>
            <v-flex xs6>
              <v-subheader v-if="item.heading">{{ item.heading }}</v-subheader>
            </v-flex>
            <v-flex xs6 class="text-center">
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-flex>
          </v-layout>
          <v-list-group
            v-else-if="item.children"
            :key="item.text"
            v-model="item.model"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon
          >
            <template v-slot:activator>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>{{ item.text }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
            <v-list-item
              v-for="(child, i) in item.children"
              :key="i" 
              @click="clickMenu(child.id)">
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>{{ child.text }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item v-else :key="item.text" @click="clickMenu(item.id)">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>{{ item.text }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="blue darken-3"
      dark
      >
      <v-toolbar-title style="width: 300px" class="ml-0 pl-4">
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <span class="hidden-sm-and-down">物资供应管理</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-tooltip right>
        <template v-slot:activator="{ on }">
          <v-btn icon to="/about" v-on="on">
            <v-icon>thumb_up_alt</v-icon>
          </v-btn>
        </template>
        <span>帮助手册</span>
      </v-tooltip>
      <v-btn icon to="/">
        <v-icon>notifications</v-icon>
      </v-btn>
      <v-btn icon large @click="selectSource">
        <v-avatar size="32px" item>
          <v-img :src="require('../assets/logo.svg')" alt="苜蓿草科技"></v-img>
        </v-avatar>
      </v-btn>
    </v-app-bar>
    <!-- <v-container fluid fill-height>
      <v-layout justify-start>
        <v-img :src="require('../assets/main_bg.jpg')" />
      </v-layout>
    </v-container>-->
    <v-tooltip right>
      <template v-slot:activator="{ on }">
        <v-btn
          bottom
          color="pink"
          dark
          fab
          fixed
          right 
          @click="clickMenu('logout')"
          v-on="on"
          ><v-icon>add</v-icon>
        </v-btn>
      </template>
      <span>退出登录</span>
    </v-tooltip>
  </div>
</template>

<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    dialog: false,
    drawer: null,
    items: [
      { 
        icon: "contacts", 
        text: "物资基础数据", 
        "icon-alt": "keyboard_arrow_down",
        model: false,
        children: [
          { text: "科室管理", id: "detail1" },
          { text: "人员管理", id: "detail2" },
          { text: "期间划分调整", id: "detail3" },
          { text: "物资生产商管理", id: "detail4" },
          { text: "物资供应商管理", id: "detail5" },
          { text: "物资入出分类", id: "detail6" },
          { text: "物资目录管理", id: "detail7" },
          { text: "物资定额管理", id: "detail8" }
        ]
      },
      { icon: "history", 
        text: "物资库房管理", 
        "icon-alt": "keyboard_arrow_down",
        model: false,
        children: [
          { text: "外购入库", id: "d1" },
          { text: "其他入库", id: "d2" },
          { text: "物资移库", id: "d3" },
          { text: "物资领用", id: "d4" },
          { text: "其他出库", id: "d5" },
          { text: "物资盘存", id: "d6" },
          { text: "物资申购", id: "d7" },
          { text: "物资计划", id: "d8" },
          { text: "物资更换", id: "d9" }
        ]
      },
      { icon: "content_copy", 
        text: "在用物资管理", 
        "icon-alt": "keyboard_arrow_down",
        model: false,
        children: [
          { text: "物资报废", id: "e1" },
          { text: "物资发放", id: "e2" },
          { text: "物资权属变更", id: "e3" }  
        ] 
      },
      { icon: "keyboard", 
        text: "查询与统计",
        "icon-alt": "keyboard_arrow_down",
        model: false,
        children: [
          { text: "库存查询", id: "f1" },
          { text: "明细帐", id: "f2" },
          { text: "总帐", id: "f3" },
          { text: "入出汇总表", id: "f4" },
          { text: "分科汇总表", id: "f5" },
          { text: "分类统计表", id: "f6" },
          { text: "消耗汇总表", id: "f7" },
          { text: "超储短缺分析", id: "f8" },
          { text: "效期报警分析", id: "f9" },
          { text: "滞用报警分析", id: "f10" },
          { text: "一次性卫生用品发放表", id: "f11" },
          { text: "明细表", id: "f12" },
          { text: "调拨汇总表", id: "f13" },
          { text: "外购汇总表", id: "f14" },
          { text: "科室领用汇总表", id: "f15" },
          { text: "用途分类汇总表", id: "f16" },
          { text: "科室物资查询", id: "f17" }
        ] 
       },
      {
        icon: "keyboard_arrow_up",
        "icon-alt": "keyboard_arrow_down",
        text: "供应室查询与统计",
        model: false,
        children: [
          { text: "供应室物资明细帐", id: "detail_reg" },
          { text: "供应室物资总帐", id: "detail_cash" },
          { text: "供应室工作量报表", id: "detail_chk" }
        ]
      },
      {
        icon: "settings",
        "icon-alt": "keyboard_arrow_down",
        text: "管理与维护",
        model: false,
        children: [
          { text: "物资参数设置", id: "mg_dict",icon: "content_copy" },
          { text: "物资月结管理", id: "mg_analyse", icon: "history"}
        ]
      },
      { icon: "help", text: "退出登录", id: "logout" }
    ]
  }),
  methods: {
    clickMenu(tstr) {
      console.log("点击=" + tstr);
      if (tstr === "logout") {
        localStorage.removeItem("user");
        this.$router.push({ path: "/login" });
      }
      switch (tstr)
      {
        case "out_reg":
          this.$router.push({ path: "/outreg" });
          break;
        case "out_cash":
          this.$router.push({ path: "/outcash" });
          break;
        case "out_chk":
          this.$router.push({ path: "/outchk" });
          break;
        case "out_receipt":
          this.$router.push({ path: "/outreceipt" });
          break;
        case "detail_reg":
          this.$router.push({ path: "/detailreg" });
          break;
        case "detail_cash":
          this.$router.push({ path: "/detailcash" });
          break;
        case "detail_chk":
          this.$router.push({ path: "/detailchk" });
          break;
        case "detail_undo":
          this.$router.push({ path: "/detailundo" });
          break;
        case "detail_op":
          this.$router.push({ path: "/detailop" });
          break;
        case "mg_dict":
          this.$router.push({ path: "/mgdict" });
          break;
        case "mg_analyse":
          this.$router.push({ path: "/mganalyse" });
          break;
        case "mg_invoice":
          this.$router.push({ path: "/mginvoice" });
          break;
        default:
          localStorage.removeItem("user");
          this.$router.push({ path: "/login" });
      }
    },

    selectSource() {
      window.location.href = "http://www.cloveropen.com";
    }
  }
};
</script>
