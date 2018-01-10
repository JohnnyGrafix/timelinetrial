<template>
  <div>
    <nav>
      <draggable-timeline :start="startdate" end="2017" :value="date" v-on:datechanged="dateChanged"></draggable-timeline>
      <img class="timeline" src="./assets/timeline.svg" alt="timeline">
      <!-- <ul>
        <li v-for="item,i in items" v-on:click="switchItem(i)" :class="{active: i==current}" :style="{backgroundColor: item.props.color}">
        {{ item.props.name }}
        </li>
      </ul> -->
    </nav>
    <main>
      <ul>
        <li v-for="item in currentItems">
          <h3>{{ item.props.title }}</h3>
          <div class="role">
            {{ item.status }} ({{ item.date.label }})
              </div>
          <div class="person">
            <div class="text">
              <div class="description"> {{ item.props.description }}
			  </div>
			</div>  
			  <div class="portrait">
              <img :src="'assets/'+item.image" />
            </div>
          </div>
        </li>
      </ul>
    </main>
    
  </div> 
</template>

<script>
  module.exports = {
    data: function data() {
      return {
        date: 1953
      }
    },
    props: ['items'],
    components: {
      'draggable-timeline': httpVueLoader('./draggable-timeline.vue')
    },
    computed: {
      startdate: function startdate() {
        return d3.min(this.items, function(item) {
          return parseInt(item.date.start)
        });
      },
      currentItems: function currentItems() {
        var date = this.date
        return this.items.filter(function(item) {
          return parseInt(item.date.start) <= date && parseInt(item.date.end) > date
        });
      }
    },
    mounted: function mounted() {
    },
    methods: {
      dateChanged: function dateChanged(val) {
        this.date = val
      }
    }
  }
</script>

<style scoped>
  ul li {
    list-style-type: none;
  }

  .chinese {
    margin-left: 4px;
    font-size: 14px;
    color: white;
  }

  .person {
    width: 100%;
    height: 100%;
    vertical-align: middle;
  }

  .text {
    display: inline-block;
    height: 100%;
    width: 59%;
    vertical-align: middle;
  }
  

  .role {
    font-size: 15px;
    color: #8f8f8f;
    font-weight: bold;
    text-transform: uppercase;
  }

  .description {
    display: block;
  }

  .portrait {
    display: inline-block;
    width: 40%;
    text-align: center;
  }

  .person img {
    width: 95%;
    vertical-align: middle;
  }

  .timeline {
    text-align: center;
    width: 100%;
  }


  @media screen and (max-width: 450px) {
    .text {
      display: block !important;
      width: 100% !important;
    }

    .portrait {
      display: block !important;
      width: 100% !important;
    }

    ul li {
      margin-top: 25px !important;
    }

    .person img {
      width: 75% !important;
      margin: 15px 0;
    }
  }
</style>