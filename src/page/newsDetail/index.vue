<template lang="html">
  <div class="columnDetail centerBox">
    <div class="cd-cont">
      <div class="cd-info">
        <p class="cd-title">{{curData.title}}</p>
        <div>
          <span>{{curData.from}}</span>
          <p>
            <span>{{curData.dateline | showTime}}</span>
            <span>{{curData.readcount}}人{{$t("news.2")}}</span>
          </p>
        </div>
      </div>

      <div v-html="curData.text"></div>
    </div>

    <div class="recommendNews">
      <h4>{{$t('news.1')}}</h4>
      <ul>
        <li v-for="item in recommend">
          <router-link :to="{ name: 'newsDetail', params: {id: item.id} }">
            <h5>{{item.title}}</h5>
            <p>{{item.desc | strFilter}}</p>
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
    import {
        cutStr,
        timeStringStyle
    } from '../../assets/js/mUtils'
    export default {
        data() {

            return {
                recommend: [],
                curData:{},//当前显示数据
            }
        },
        mounted() {
            this.getColumnDetail();
        },
        filters: {
            timeStringFilter(val){
              // val = val -  parseInt(Math.random()*(7*24*60*60*1000))
              return timeStringStyle(val)
            },
            strFilter(val){
              return cutStr(val, 90)
            },
            showTime(time) {
              time = parseInt(time);
              time = time > 1e12 ? time : time * 1000;
              let date = new Date(time),
                month = date.getMonth() + 1,
                day = date.getDate();
                month = (month < 10) ? "0" + month : month;
            		day = (day < 10) ? "0" + day : day;

                return month + "-" + day;
            }
        },
        methods: {
            getColumnDetail() {
              this.http('get', 'common/common/advisoryDetails', {
                id: this.$route.params.id
              }, data => {
                  this.recommend = data.recommend;
                  this.curData = data.data;
              });
            }
        },
        beforeRouteUpdate(to, from, next) {
            next();
            this.getColumnDetail();
        }
    }
</script>

<style lang="less">
    .columnDetail {
        padding-top: 50px;
        display: flex;
        align-items: flex-start;

        .cd-cont{
          width: 830px;
          padding: 50px 40px;
          margin: 0 73px 50px 0;
          background: #fff;
          border-radius: 4px;
          .cd-info{
            margin-bottom: 36px;
            .cd-title{
              font-size: 20px;
              font-weight: bold;
              color: #333;
            }
            &>div{
              display: flex;
              justify-content: space-between;
              margin-top: 17px;
              span{
                font-size: 16px;
                color: #666;
                &:last-child{
                  margin-left: 50px;
                  color: #999;
                }
              }
            }
          }
        }
        .recommendNews {
            width: 300px;
            padding: 40px;
            background: #fff;
            border-radius: 4px;
            h4 {
                padding: 10px 0;
                margin: 0;
                border-bottom: 1px solid #DEDEDE;
                font-size: 18px;
                line-height: 18px;
                color: #333;
                font-weight: 400;
            }
            ul {
                li {
                    a {
                        h5 {
                            margin: 0;
                            height: 18px;
                            width: 100%;
                            line-height: 18px;
                            font-size: 18px;
                            overflow: hidden;
                            overflow: hidden;
                            text-overflow: ellipsis;
                            white-space: nowrap;
                            font-weight: 400;
                            color: #333;
                            margin-top: 20px; // transition: all .2s;
                            &:hover {
                                color: #FC583D
                            }
                        }
                        p {
                            font-size: 14px;
                            color: #999;
                            margin-top: 8px;
                            line-height: 24px;
                            overflow: hidden;
                        }
                    }
                }
            }
        }
    }
</style>
