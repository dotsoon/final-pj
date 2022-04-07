<template>
<div>
    <h1>결과</h1>
    <div class="container">
      <img :src='img' id="tableBanner" class="result" />
      <h2> {{ result }}</h2>     
    </div>
    <div class = "like">
      <span> <font-awesome-icon icon="fa-solid fa-heart" color=tomato font-size=35px class=fa-beat v-on:click="add()" /> 
       {{likecnt}} </span><br>
    </div>
    <div class="sns">
      공유하기<br>
      <img class="kakao_btn" src="@/assets/kakao.png" @click="kakaoLink" />
      <img class="twitter_btn" src="@/assets/twitter.png" @click="twitterLink"/>
      <img class="facebook_btn" src="@/assets/facebook.png" @click="facebookLink"/>
      <img class="link_btn" src="@/assets/link.png" @click="linkCopy"/>
    </div>
</div>
</template>


<script>  
  export default {
    name: 'App',
    data() {
      return{
        img: JSON.parse(localStorage.getItem('image')),
        likecnt : 0,
        result: JSON.parse(localStorage.getItem('result')),
      }
    },

    methods:{
      add(){
        if (this.likecnt == "0")
          this.likecnt ++;
        else
          this.likecnt --;
      },
      kakaoLink () {
          window.Kakao.Link.sendDefault({
            objectType:'feed',
            content:{
              title:'강아지 정상/비만 판별기',
              description:'우리 강아지는 정상일까 비만일까',
              imageUrl:'http://35.76.37.170/img/Dog.9d7ce6f7.jpg',
              link:{
                mobileWebUrl:
                  'https://developers.kakao.com',
                webUrl:
                  'https://developers.kakao.com'
            }},
          buttons: [
            {
              title: '웹으로 보기',
              link: {
                mobileWebUrl: '카카오공유하기 시 클릭 후 이동 경로',
                webUrl: '카카오공유하기 시 클릭 후 이동 경로',
              },
            },
          ],
        })
        },
        twitterLink(){
          var sendText = "강아지 정상/비만 판별기"; 
          var sendUrl = "http://3.113.137.203"; 
          window.open("https://twitter.com/intent/tweet?text=" + sendText + "&url=" + sendUrl);
        },
        facebookLink(){
          var sendUrl = "http://3.113.137.203"; 
          window.open("http://www.facebook.com/sharer/sharer.php?u=" + sendUrl);
        },
        async linkCopy() {
          try {
            await navigator.clipboard.writeText(location.href);
            console.log('Page URL copied to clipboard');
            alert('URL이 복사되었습니다.');
          } catch (err) {
            console.error('Failed to copy: ', err);
          }
        }
    }
  }
</script>

<style>
.container {
  min-height: 300px;
  width: 500px;
  text-align:center;
  margin: 0 auto;
  margin-top:50px;
}
.result {
  height: 300px;
  padding: 20px;
  box-shadow: 0 0.625rem 1.25rem #0000001a;
  border-radius: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.like{
  margin-top:50px;
  margin-bottom:40px;
  font-size:30px;
}
.sns{
  padding-top:25px;
  padding-bottom:100px;
  text-align:center;
  font-size:30px;
}
img{
  margin-top:20px;
  height:50px;
}
img[id="tableBanner"]{
  display: center;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 50%;
}

</style>