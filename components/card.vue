<template>
    <div class="card__wrapper">
        <div v-for="(item, i) in subscribers" :key="i" class="item">
        <div class="title">{{item.Name}}</div>
        <div class="email">{{item.Email}}</div>
        <div class="subscriber"><img src="/static/images/subscriber.jpg" alt="subsciber" />{{item.SubscriberId}}</div>
      </div>
    </div>

</template>
<script>
export default {
  props: {
    subscribers:{
        type: Array,
        required: true,
        default: () => {}
    }
  }
}
</script>
<style lang="scss" scoped>
.card__wrapper {
  padding: 30px 0;
  width: clamp(270px, 100%,  940px);
  margin: auto;
  display: inline-flex;
  flex-wrap: wrap;
  gap: 15px;
}
.item {
  border: solid 1px #D6D6D6;
  border-radius: $radius;
  box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, .1);
  height: 64px;
  padding: 40px 24px;
  text-align: left;
  width: 252px;
  @media (max-width: 1040px) {
    width: calc(33% - 60px);
  }
  @media (max-width: 700px) {
    width: calc(50% - 60px);
  }
  @media (max-width: 400px) {
    width: calc(100%);
  }
}
.title {
  font-size: 18px;
  font-weight: 700;
}
.email {
  font-size: 14px;
  color: #636363;
  line-height: 16px;
  padding: 3px 0;
}
.subscriber {
  font-size: 14px;
  color: #A3A3A3;
  line-height: 16px;
  padding-top: 3px;
  img {
    height:12px;
    width: 15px;
    padding-right: 5px;
  }
}
</style>