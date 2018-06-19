<template>
  <div class="hello">
    <div class="holder">

      <form @submit.prevent="addSkill" autocomplete="off">
        <input type="text" placeholder="Enter a skill" v-model="skill" v-validate="'min:2'" name="skill">
        
        <transition name="alert-in" enter-active-class="animated flipInX" leave-active-class="animated flipOutX">
          <div class="alert" v-if="errors.has('skill')"> {{ errors.first('skill') }} </div>
        </transition>

        <input type="checkbox" id="checkbox" v-model="checked" v-show="showCheck">
      </form>
      <!-- {{ skill }} -->

      <ul>
        <transition-group enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
          <li v-for="(data, index) in skills" :key='index'>
            {{ data.skill }}
            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
          </li>
        </transition-group>
      </ul>

      <!-- using vbind on class attribute. attach class only if attribute is true-->
      <div v-bind:class="alertObject"></div>

      <div v-bind:style="{ backgroundColor: bgColor, width: bgWidth, height: bgHeight }"></div>

      <p>These are your sk1llz.</p>

    </div>
  </div>
</template>


<script>
import Skill from "./Skill.vue";

export default {
  name: 'Skills',
  components: {
    'skill': Skill
  },
  data() {
    return {
      delColor: 'grey',
      showCheck: false,
      checked: false,
      skill: '',
      skills: [],
      alertObject: {
        alert: false,
      },
      bgColor: 'blue',
      bgWidth: '100%',
      bgHeight: '30px'
    }
  },
  methods: {
    addSkill() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.skills.push({skill: this.skill})
        }
      })
    },
    remove(id) {
      this.skills.splice(id,1);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scope>
  @import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1";
  @import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

  .alert {
    background-color: yellow;
    width: 100%;
    height: 30px;
  }
  .another-class {
    border: 5px solid black;
  }

  .holder {
    background: #fff;
  }

  ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  
  ul li {
    padding: 20px;
    font-size: 1.3em;
    background-color: #E0EDF4;
    border-left: 5px solid #3EB3F6;
    margin-bottom: 2px;
    color: #3E5252; 
  }

  p {
    text-align:center;
    padding: 30px 0;
    color: gray;
  }

  .container {
    box-shadow: 0px 0px 40px lightgray;
  }
  input {
    width: calc(100% - 40px);
    border: 0;
    padding: 20px;
    font-size: 1.3em;
    background-color: #323333;
    color: #687F7F;
  }

  .alert {
    background: #fdf2ce;
    font-weight: bold;
    display: inline-block;
    padding-top: 10px;
    margin-top: 0px;
  }

  .alert-in-enter-active {
    animation: bounce-in .5s;
  }
  .alert-in-leave-active {
    animation: bounce-in .5s reverse;
  }

  .fa-minus-circle:hover {
    color: red;
  }

  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.2);
    }
    100% {
      transform: scale(1);
    }
  }
  i {
    float: right;
  }

</style>
