<template>
<div>
    <h1>결과</h1>
    <br><br><span>선택한 견종 : {{checked}}</span><br>
    <div class="container">
      <img :src='img' id="tableBanner" class="result" />
      <h1> {{ result }}</h1>     
    </div>
    <div class = "like">
      <span> <font-awesome-icon icon="fa-solid fa-heart" color=tomato font-size=35px class=fa-beat v-on:click="add()" /> 
       {{likecnt}} </span><br>
    </div>
    <div class="sns">
      공유하기<br><br>
      <img class="link_btn" src="@/assets/kakao.png" @click="kakaoLink" />
      <img class="link_btn" src="@/assets/twitter.png" @click="twitterLink"/>
      <img class="link_btn" src="@/assets/facebook.png" @click="facebookLink"/>
      <img class="link_btn" src="@/assets/link.png" @click="linkCopy"/>
    </div>
        <div class="diet">
      반려견 체중 관리법<br>
      <a href ="https://petdoc.co.kr/ency/224" target='_blank' >
        <img class="diet" src="@/assets/diet.png" id="diet"/></a><br>
    </div>
    <div class="food">
      다이어트 사료 추천<br>
      <a href ="https://www.google.com/search?q=%EB%B0%98%EB%A0%A4%EA%B2%AC+%EB%8B%A4%EC%9D%B4%EC%96%B4%ED%8A%B8+%EC%82%AC%EB%A3%8C&ei=ht9LYv7qB4X7-QaN_qH4CQ&ved=0ahUKEwj-xdHbo_z2AhWFfd4KHQ1_CJ8Q4dUDCA4&uact=5&oq=%EB%B0%98%EB%A0%A4%EA%B2%AC+%EB%8B%A4%EC%9D%B4%EC%96%B4%ED%8A%B8+%EC%82%AC%EB%A3%8C&gs_lcp=Cgdnd3Mtd2l6EAMyBggAEAcQHjoHCAAQRxCwAzoECAAQDToICAAQDRAFEB5KBAhBGABKBAhGGABQlwRY7wlg1wpoAnABeAGAAXGIAeQGkgEDMC44mAEAoAEByAEFwAEB&sclient=gws-wiz" target='_blank'>
        <img class="food" src="@/assets/food.png" target='_blank' id="food" /></a>
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
        checked: JSON.parse(localStorage.getItem('dog_breed')),
      }
    },

    methods:{
      add(){
        if (this.likecnt == "좋아요") {         
          this.$http.put('http://35.76.37.170:8980/dogobesitytest/testresult/', {
                        image: JSON.parse(localStorage.getItem('image_name')),
                    })
                .then(
                    (res) => {
                        if (res.status == 200 ) {
                          this.likecnt = "감사합니다!"                          
                        } else {
                            alert(res.data.message);                                    
                        }
                    
                    },
                    (err) => {
                        alert(res.data.message)
                    }
                )
                .catch((err) => {
                    alert(err);
                })
        } else {
          alert('좋아요는 한번만 가능합니다!')
        }
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
          var sendUrl = "http://35.76.37.170"; 
          window.open("https://twitter.com/intent/tweet?text=" + sendText + "&url=" + sendUrl);
        },
        facebookLink(){
          var sendUrl = "http://35.76.37.170"; 
          window.open("http://www.facebook.com/sharer/sharer.php?u=" + sendUrl);
        },
        linkCopy() {
          	var url = '';
	          var textarea = document.createElement("textarea");
	          document.body.appendChild(textarea);
	          url = window.document.location.href;
	          textarea.value = url;
	          textarea.select();
	          document.execCommand("copy");
	          document.body.removeChild(textarea);
	          alert("URL이 복사되었습니다.")
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
.link_btn{
  margin-left:10px;
}
.tableBanner{
  display: center;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 50%;
}
.diet{
  font-size:30px;
  margin-top:20px;
  margin-bottom:100px;
  height:100px;
}
.food{
  font-size:30px;
  height:100px;
}

</style>