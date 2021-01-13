<template>
  <div class="hello">
        <body>
        <nav class="navbar navbar-expand-md navbar-dark bg-dark">
            <a class="navbar-brand" href="#">My Movie List</a>
        </nav>
        <div class="container">
            <form>
                <div class="form-group">
                    <label for="input-title-ch">Title in Chinese</label>
                    <input type="text" v-model="HECName" class="form-control" placeholder="金牌特務">
                </div>
                <div class="form-group">
                    <label for="input-title-eng">Title in English</label>
                    <input type="text" v-model="NECName" class="form-control" placeholder="Kingsman">
                </div>
                <div class="form-group">
                    <label for="input-intro">Intro</label>
                    <input type="text" v-model="Description" class="form-control" placeholder="是一部於2015年上映，由英國、美國合拍的諜報喜劇動作片...">
                </div>
                <div id="button-insert" class="btn btn-primary" @click="NewAction2()">Insert</div>
            </form>
            <!-- <ul id="list-movie" class="list-group">
            </ul> -->
            <hr>
            <ul class="list-group" v-for="item in list" :key="item.ch_name" >
                <!-- <li class="list-group-item">{{item.ch_name}}</li> -->
                <!-- <li class="list-group-item">{{item.eng_name}}</li> -->
                <!-- <li class="list-group-item">{{item.intro}}</li> -->
                <li class="list-group-item">
                    <h3>{{item.ch_name}}</h3>
                    <h4>{{item.eng_name}}</h4>
                    <div class="movie-intro">{{item.intro}}</div>
                </li>
            </ul>
        </div>
    </body>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
        HECName: '',
        NECName: '',
        Description: '',
        list: []
    }
  },
  created() {
    this.$http.get('/herokuapp/api/movie/list.php/api/movie/list.php')
    .then(async (response) => {
      //console.log(response.data)
      this.list = response.data;
    })
  },
  methods:{
    NewAction2: function(){
        var mes = "";
        if(this.HECName == ""){
            mes+="請填寫中文標題\r\n";
        }else if(this.HECName.length > 50){
            mes+="中文標題過長\r\n";
        }
        var NECName = this.NECName;
        NECName=NECName.replace(/\$/g,"");
        NECName=NECName.replace(/\&/g,"");
        NECName=NECName.replace(/\%/g,"");
        NECName=NECName.replace(/\^/g,"");
        NECName=NECName.replace(/\*/g,"");
        if(NECName == ""){
            mes+="請填寫英文標題\r\n";
        }else if(NECName.length < 10){
            mes+="英文標題過短\r\n";
        }else if(NECName.length > 100){
            mes+="英文標題過長\r\n";
        }
        if(this.Description == ""){
            mes+="請填寫簡介\r\n";
        }else if(this.Description.indexOf("Intro")<0){
            mes+="簡介請'Intro'做為開頭\r\n";
        }else if(this.Description.length<10){
            mes+="簡介長度過短\r\n";
        }else if(this.Description.length>255){
            mes+="簡介長度過長\r\n";
        }
        if("" != mes){
            this.$swal('<style="color:red;>'+mes+'</style>');
        }else{
            this.list.push({
                ch_name:this.HECName,
                eng_name:this.NECName,
                expectation: '',
                intro: this.Description,
                movie_id: '',
                poster_url: '',
                release_date: '',
                trailer_url: ''
            });
            this.HECName = "";
            this.NECName = "";
            this.Description = "";
        }
    }
  }
}
</script>

< lang = "scss">
  @import "~bootstrap/scss/bootstrap";
</>
