<template lang="jade">
.component
  .ui.container

    h2.ui.sticky.header.m-title.thin-only(:data-name="name")
      | {{ label }}

    .desc.ui.basic.center.aligned.segment.fat-only
      i.quote.left.icon
      | {{ desc }} 
      i.quote.right.icon

    .ui.four.column.grid.stackable

      .box.column(v-if="list.length == 0")
        .box-inner.ui.segment
          | nothing

      .box.column(v-for="t in list")

        router-link.box-inner.ui.segment(:to="'/topic/' + t.routeName")
          
          img(:src ="t.cover || 'http://lorempixel.com/320/240/sports'")

          h3.ui.header {{ t.title }}

          .progress_bar
            .progress(v-bind:style="{ width: (t.progress / t.total * 100) + '%' }")

          .progress_text.ui.top.right.attached.teal.large.label(v-if="t.status==='討論中'") 還有{{Math.floor(t.total - t.progress)}}天
          .progress_text.ui.top.right.attached.blue.large.label(v-else) 討論已結束

</template>

<script>
  export default {
    name: 'box',
    props: ['list', 'desc', 'label', 'name'],
    data() {
      return {
        mTop: 0 /* top offset of m-title */
        // onMobile:false
      }
    },
    updated:function(){
      let el = ".m-title[data-name='"+this.name+"']"
      this.mTop = $(el).offset().top /* m-title position */
      
      // this.handleResize();
    },
    mounted:function(){
      // window.addEventListener('resize', this.handleResize);

      /* make mobile's m-title sticky to its own content */
      // let self = this /* save vm for later use */
      // let btn_name = this.name
      // $('.m-title.sticky[data-name="'+btn_name+'"]').sticky({
      //   observeChanges: true,
      //   onStick       : function(){
      //     self.$emit('stickTo', btn_name)
      //   }
      // })

      /* bind event scroll to window */
      // window.addEventListener('scroll', this.mTitleHitEvent);
      $(window).scroll(this.mTitleHitEvent)

    },
    methods:{
      // handleResize: function(){
      //   if(typeof window !== 'undefined')
      //       this.onMobile = window.innerWidth < 768;
      //   else
      //       this.onMobile = false;
      // }

      mTitleHitEvent: function() {
        let btn_name = this.name
        let el = "#mobile-step a[href='#"+btn_name+"']"
        let mTop = this.mTop
        let wScroll = $(window).scrollTop() /* length window has scrolled */
        if ( wScroll>mTop ){
          this.$emit('stickTo', btn_name) /* sent event trigger to parent comp */
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
@import "../sass/global.scss";

.desc {
  width: 100%;
  margin-top: -1em;
}

.box {
  display: flex !important;
  .box-inner {
    box-shadow: 0 3px 1em hsla(0,0,0%,0.1);
    display: flex;
    flex-flow: column nowrap;
    img {
      flex: 1 1;
      max-width: 100%;
      height: auto;
    }
    .progress_bar {
      background-color: #CCC;
      padding: 0;

      .progress {
        height: 5px;
        background-color: lightcoral;
      }
      margin-bottom: 4px;
    }
  }
}


/********************************* mobile view */
// .m-step-title {
//   background-color: #E6E6E6;
//   text-align: left;
//   padding: 1rem;
//   font-size: 1.3rem;
//   margin-right: -2rem;
//   position: absolute;
//   z-index: 1;
//   width: 120%;
// }

// .thin-only {
//   margin-top: -1em;
// }

// .m-context {
//   padding-top: 50px;
// }

.m-title {
  // text-align: left;
  background-color: #E6E6E6;
  // margin-right: -1rem;
  padding: .5em;
}


</style>