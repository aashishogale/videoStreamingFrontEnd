<template>
  <div class>
    <div class>
      <md-toolbar style="background:#403a3a" class="md-medium md-toolbar-offset">
        <md-avatar class="md-avatar-icon md-large">
          <img class="logo" src="../assets/images.png" />
        </md-avatar>

        <h3 class="md-title title" style="flex: 1; color: white;
 ">Video Streaming</h3>
        <md-avatar class="md-avatar-icon md-large">
          <img class="logo" src="../assets/logo.png" />
        </md-avatar>
      </md-toolbar>
    </div>
    <div class="md-layout">
      <v-treeview
        class="md-layout-item md-size-20 tree"
        v-model="tree"
        :items="items"
        :active="active"
        hoverable
        activatable
        open-on-click
        shaped
        item-key="name"
      >
        <template slot="label" slot-scope="{ item }">
          <span @click="test(item)">{{item.name}}</span>
        </template>
      </v-treeview>
      <VideoPlayer v-bind:video="video" class="md-layout-item md-size-77" />
    </div>
  </div>
</template>



<script>
import VideoPlayer from "./VideoPlayer";
// import TestVideo from "../assets/test2.mp4";
import TestVideo2 from "../assets/test.mp4";
import { HTTP } from "../services/httpService";
var items = []

HTTP.get('/getFellowshipVideoData')
  .then(response => {
    var node_dict = {}
    var tech_list = response.data["data"]
    var count = 1
    for (var i=0; i<tech_list.length;i++){
        var tech_data = tech_list[i]
        node_dict = {"id":count,"name":tech_data["core"]}
        var stage_list = tech_data["stages"]
        if (typeof stage_list != "undefined"){
        var stage_array = []
        count ++;
        for (var j=0; j<stage_list.length;j++){
          var stage_data = stage_list[j]
          var stage_dict = {"id":count, "name": stage_data["name"] }
          stage_array.push(stage_dict)
          
          var topic_list = stage_data["topics"]
          count++
          if (typeof topic_list != "undefined"){
          var topic_array = []
          for (var k=0; k<topic_list.length; k++) {
              var topic_dict = topic_list[k]
              var topic_node = {"id": count,"name": topic_dict["topic"],"video": "http://127.0.0.1:8000"+topic_dict["video_local_path"]}
              topic_array.push(topic_node)
            
              count ++

          }
          stage_dict["children"] = topic_array
          }
        }
        }
        node_dict["children"]=stage_array
        items.push(node_dict)



    }

    
  })
export default {
  components: {
    VideoPlayer
  },
  data: () => ({
    video: TestVideo2,
    active: [],
    tree: [],
    // items: [
    //   {
    //     id: 1,
    //     name: "Applications",
    //     children: [
    //       { id: 2, name: "Calendar" },
    //       { id: 3, name: "Chrome" },
    //       { id: 4, name: "Webstorm" }
    //     ]
    //   },
    //   {
    //     id: 5,
    //     name: "Documents",
    //     children: [
    //       {
    //         id: 6,
    //         name: "vuetify",
    //         children: [
    //           {
    //             id: 7,
    //             name: "src",
    //             children: [
    //               { id: 8, name: "index", video: TestVideo },
    //               { id: 9, name: "bootstrap", video: TestVideo2 }
    //             ]
    //           }
    //         ]
    //       },
    //       {
    //         id: 10,
    //         name: "material2",
    //         children: [
    //           {
    //             id: 11,
    //             name: "src",
    //             children: [
    //               { id: 12, name: "v-btn" },
    //               { id: 13, name: "v-card" },
    //               { id: 14, name: "v-window" }
    //             ]
    //           }
    //         ]
    //       }
    //     ]
    //   },
    //   {
    //     id: 15,
    //     name: "Downloads",
    //     children: [
    //       { id: 16, name: "October" },
    //       { id: 17, name: "November" },
    //       { id: 18, name: "Tutorial" }
    //     ]
    //   },
    //   {
    //     id: 19,
    //     name: "Videos",
    //     children: [
    //       {
    //         id: 20,
    //         name: "Tutorials",
    //         children: [
    //           { id: 21, name: "Basic layouts" },
    //           { id: 22, name: "Advanced techniques" },
    //           { id: 23, name: "All about app" }
    //         ]
    //       },
    //       { id: 24, name: "Intro" },
    //       { id: 25, name: "Conference introduction" }
    //     ]
    //   }
    // ],
    items : items,
  }),
  methods: {
    test(item) {
      this.$log.info("data at 94: ", item.video);
      if (item.video) this.video = item.video;
    }
  }



  
};
  
</script>

<style scoped>
.v-treeview--active {
  font-size: 20px;
}

.tree {
  height: 100vh;
  overflow-y: auto;
  font-size: 18px;
  letter-spacing: 0.08em;
  /* background: #352d2d; */
  /* color: white; */
  cursor: pointer;
}

.logo {
  height: 33px;
}

.title {
  font-size: 24px;
  font-weight: 500;
  letter-spacing: 0.07em;
  color: white;
  font-weight: 600;
}
</style>