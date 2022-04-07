<template>
<div>
    <h1>결과</h1>
    <br><br><span>선택한 견종: {{checked}}</span><br>
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
      <img class="link_btn" src="@/assets/kakao.png" @click="kakaoLink" />
      <img class="link_btn" src="@/assets/twitter.png" @click="twitterLink"/>
      <img class="link_btn" src="@/assets/facebook.png" @click="facebookLink"/>
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
        checked: JSON.parse(localStorage.getItem('dog_breed')),
      }
    },

    methods:{
      add(){
        if (this.likecnt == "좋아요") {         
          this.$http.post('http://3.113.137.203:8900/dogobesitytest/testresult/', {
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
          var sendUrl = "http://3.113.137.203"; 
          window.open("https://twitter.com/intent/tweet?text=" + sendText + "&url=" + sendUrl);
        },
        facebookLink(){
          var sendUrl = "http://3.113.137.203"; 
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

</style>