<template>
<div class="wrap">
  <div class="left">
    <div class="title">
      通圓書堂 體性命理
    </div>
    <br>
    <form class="form2">
      <div>
          <label for="name">姓銜:</label>
          <input type="text" v-model="name" />
      </div>
      <div>
          <label for="name">生年月日時:</label>
          <select>
              <option v-for= "option in UY" :key= "option.text">
              {{ option.text }}
              </option>
          </select>
          <input type="Birth_date" v-model="Birth_date" />
      </div>
      <div>
          <label for="name">性別:</label>
          <input type="gender" v-model="gender" />
      </div>
      
      <div>
          <label for="name">干支:</label>
          <input type="text" v-model="typed_ganzi" />
      </div>
      <button @click="[change_ganzi(),cal_gisae(12),cal_5hang_cons(),get_gender()]">입력완료</button>
      <br>test: 신미을미신사을미<br>
      test: 임신갑진임자경자<br>
      test:임자임자병술무술
    </form>
    <br>
    <form class="form2">
      <div>
          <label for="name">旺勢:</label>
          <input type="text"/>
      </div>
      <div>
          <label for="name">用神:</label>
          <input type="text"/>
      </div>
      <div>
          <label for="name">體性格局:</label>
          <select>
            <option v-for= "option in GeokGuk" :key= "option.text">
            {{ option.text }}
            </option>
          </select>
      </div>
    </form>
  </div>
  <div class="center">
    <div class="four_pillar">
      <div class="single_pilar" v-if= "big_luck[0].big_luck_sky != 'o' ">
        <div class="single_letter">
          {{ big_luck[0].big_luck_sky }}
        </div>
        <div class="single_letter">
          {{ big_luck[1].big_luck_ear }}
        </div>
        <div class="amjang_letter" v-for= "j in [0,1] " :key="'B'+j">
          <span class="amjang_letter_1">{{big_luck[2].big_luck_Amzang[j][0]}}</span>
          <span class="amjang_letter_2">{{big_luck[2].big_luck_Amzang[j][1]}}</span>
          <span class="amjang_letter_2">{{big_luck[2].big_luck_Amzang[j][2]}}</span>
        </div>
        <div class="amjang_letter" v-if = "big_luck[2].big_luck_Amzang.length > 2">
          <span class="amjang_letter_1">{{big_luck[2].big_luck_Amzang[2][0]}}</span>
          <span class="amjang_letter_2">{{big_luck[2].big_luck_Amzang[2][1]}}</span>
          <span class="amjang_letter_2">{{big_luck[2].big_luck_Amzang[2][2]}}</span>
        </div>
        <div class="amjang_letter" v-else>
          &nbsp; 
        </div>
      </div>
      <div class="single_pilar" v-else>
        <div class="single_letter">
          &nbsp;
        </div>
        <div class="single_letter">
          &nbsp;
        </div>
        <div class="amjang_letter" v-for= "j in [0,1,2] " :key="'B'+j">
          &nbsp;
        </div>    
      </div>
      <div class="single_pillar" v-for = "k in [0,2,4,6]" :key="k">
        <div class="single_letter"  v-for = "i in [k,k+1]" :key="i">
          {{ganzi[i].hanja }}
        </div>
        <div class="amjang_letter" v-for= "j in [1,2] " :key="'A'+j">
          <span class="amjang_letter_1">{{ganzi[k+1].amjang[j][0]}}</span>
          <span class="amjang_letter_2">{{ganzi[k+1].amjang[j][1]}}</span>
          <span class="amjang_letter_2">{{ganzi[k+1].amjang[j][2]}}</span>
        </div>
        <div class="amjang_letter" v-if = "ganzi[k+1].amjang.length > 3">
          <span class="amjang_letter_1">{{ganzi[k+1].amjang[3][0]}}</span>
          <span class="amjang_letter_2">{{ganzi[k+1].amjang[3][1]}}</span>
          <span class="amjang_letter_2">{{ganzi[k+1].amjang[3][2]}}</span>
        </div>
        <div class="amjang_letter" v-else>
          &nbsp; 
        </div>
      </div>
    </div>
    <br>
    <div class="pentagon">
      <span v-for= "i in this.fhang_cons" :key=i 
      style="border: 1px solid black; padding: 5px;" 
      :class="fhang_color[i]">{{gisae[i].id }}</span>
        <span v-for = "k in this.fhang_cons" :key=k+5
        style="font-size: small">
          {{gisae[k].content[0]}}<br>
          {{gisae[k].content[1]}}<br>
          {{gisae[k].content[2]}}<br>
          {{gisae[k].content[3]}}<br>
          {{gisae[k].content[4]}}<br>
          {{gisae[k].content[5]}}<br>
          {{gisae[k].content[6]}}
          </span>
          <span v-for = "k in this.fhang_cons" :key=k+10
        style="font-size: medium">
          {{gisae[k].value}}
          </span>
    </div>
    <table>
      運路(운로)
      <button @click=[update_Bigluck(12),cal_gisae(12)]>원국만 보기</button>
      <tr>    
        <td v-for = "i in [11,10,9,8,7,6,5,4,3,2,1,0]" :key="i">
          <button @click=[update_Bigluck(i),cal_gisae(i)]>
            {{ get_BigLuck()[i][0][0] }}<br>
            {{ get_BigLuck()[i][0][1] }}
          </button>
          <br>
          <select>
            <option v-for= "option in GH" :key= "option.text">
            {{ option.text }}
            </option>
          </select>
        </td>
      </tr>
    </table>
    
    
  </div>
  <div class="right">
    <div class="v1">
      {{Birth_date}}&nbsp;{{hanja_gender}}&nbsp;{{ganzi[7].hanja}}時
    </div>
  </div>
 </div>
</template>

<script>
export default {
  data () {
    return {
      fhang_cons: [],
      fhang_color: ["green","red","yellow","white","black"],
      typed_ganzi: '신미을미신사을미',
      Birth_date: " ",
      ganzi: [
        {id: 0, hanja: "o", value: "o", umyang: "o", ten_shin: ""}, //연간 0
        {id: 1, hanja: "o", value: "o", amjang: [["o","o","o"],["o","o","o"],["o","o","o"],["o","o","o"]] },//연지 1
        {id: 2, hanja: "o", value: "o", umyang: "o", ten_shin: ""}, //월간 2
        {id: 3, hanja: "o", value: "o", amjang: [["o","o","o"],["o","o","o"],["o","o","o"],["o","o","o"]] },//월지 3
        {id: 4, hanja: "o", value: "o", umyang: "o", ten_shin: ""}, //일간 4
        {id: 5, hanja: "o", value: "o", amjang: [["o","o","o"],["o","o","o"],["o","o","o"],["o","o","o"]]  },//일지 5
        {id: 6, hanja: "o", value: "o", umyang: "o", ten_shin: ""}, //시간 6
        {id: 7, hanja: "o", value: "o", amjang: [["o","o","o"],["o","o","o"],["o","o","o"],["o","o","o"]]  },//시지 7
        {id: 9, hanja: "o", value: "o", amjang: [["o","o","o"],["o","o","o"],["o","o","o"],["o","o","o"]]  }, //월지 한번더 8
        {id: 11, hanja: "o", value: "o", amjang: [["o","o","o"],["o","o","o"],["o","o","o"],["o","o","o"]]  } //월지 한번더 9
      ],
      name: '장승표',
      gender: '양',
      hanja_gender:  '성별',
      gisae:[
        {id: '木', value: 0, content: [],id_n: 0},
        {id: '火', value: 0, content: [],id_n: 1},
        {id: '土', value: 0, content: [],id_n: 2},
        {id: '金', value: 0, content: [],id_n: 3},
        {id: '水', value: 0, content: [],id_n: 4},
      ],
      sky_char: [
        {id: '갑', hanja: "甲", value: 0, umyang: "양"},
        {id: '을', hanja: "乙", value: 0, umyang: "음"},
        {id: '병', hanja: "丙", value: 1, umyang: "양"},
        {id: '정', hanja: "丁", value: 1, umyang: "음"},
        {id: '무', hanja: "戊", value: 2, umyang: "양"},
        {id: '기', hanja: "己", value: 2, umyang: "음"},
        {id: '경', hanja: "庚", value: 3, umyang: "양"},
        {id: '신', hanja: "辛", value: 3, umyang: "음"},
        {id: '임', hanja: "壬", value: 4, umyang: "양"},
        {id: '계', hanja: "癸", value: 4, umyang: "음"},
      ],
      earth_char: [
        {id: '인', hanja: "寅", value: 0, amjang: [
          '甲丙戊',['甲',16],['丙',7],['戊',7]] },
        {id: '묘', hanja: "卯", value: 0, amjang: [
          '乙甲', ['乙',20],['甲',10]]},
        {id: '진', hanja: "辰", value: 2, amjang: [
          '戊癸乙',['戊',18],['癸',3],['乙',9]]},
        {id: '사', hanja: "巳", value: 1, amjang: [
          '丙庚戊',['丙',16],['庚',7],['戊',7]]},
        {id: '오', hanja: "午", value: 1, amjang: [
          '丁己丙',['丁',10],['己',10],['丙',10]]},
        {id: '미', hanja: "未", value: 2, amjang: [
          '己乙丁',['己',18],['乙',3],['丁',9]]},
        {id: '신', hanja: "申", value: 3, amjang: [
          '庚壬戊',['庚',16],['壬',7],['戊',7]]},
        {id: '유', hanja: "酉", value: 3, amjang: [
          '辛庚',['辛',20],['庚',10]]},
        {id: '술', hanja: "戌", value: 2, amjang: [
          '戊丁辛',['戊',18],['丁',3],['辛',9]]},
        {id: '해', hanja: "亥", value: 4, amjang: [
          '壬甲戊',['壬',16],['甲',7],['戊',7]]},
        {id: '자', hanja: "子", value: 4, amjang: [
          '癸壬',['癸',20],['壬',10]]},
        {id: '축', hanja: "丑", value: 2, amjang: [
          '己辛癸',['己',18],['辛',3],['癸',9]]},
      ],
      big_luck: [
        {big_luck_sky: 'o', umyang: '', ten_shin: '', value: 0},
        {big_luck_ear: 'o'},
        {big_luck_Amzang: [["o","o","o"],["o","o","o"],["o","o","o"],["o","o","o"]]},
      ],
      GH: [
        { text: '??'},{ text: '吉' },{ text: '平'},{ text: '凶'}
      ],
      UY: [
        { text: '양력'},
        { text: '음력'},
      ],
      GeokGuk: [
        { text: '曲直'},{ text: '養生'},{ text: '活人'},{ text: '秋收棟樑'},{ text: '炎上'},
        { text: '燥土生金 成器'},{ text: '燥土生金 光彩'},{ text: '水火旣濟'},{ text: '取金'},{ text: '取水'},
        { text: '潤下'},{ text: '稼穡'},{ text: '從革'},{ text: '洗水'}
      ]
    }
  },
  methods: {
    get_gender(){
      if (this.gender=='양')
      {
        this.hanja_gender = '乾命'
      }
      else if(this.gender =='음')
      {
        this.hanja_gender = '坤命'
      }
    },
    cal_5hang_cons(){
      if (this.ganzi[4].value==0){
        this.fhang_cons = [0,1,2,3,4]
      }
      else if(this.ganzi[4].value==1){
        this.fhang_cons = [1,2,3,4,0]
      }
      else if(this.ganzi[4].value==2){
        this.fhang_cons = [2,3,4,0,1]
      }
      else if(this.ganzi[4].value==3){
        this.fhang_cons = [3,4,0,1,2]
      }
      else if(this.ganzi[4].value==4){
        this.fhang_cons = [4,0,1,2,3]
      }
    },
    get_10shin(a,b){ ///a: 십신을 도출하고자 하는 천간, b: 해당 천간의 음양 (정편 구분을 위해)
      var ss_o = [];
      var ss_x = [];
      if (this.ganzi[4].value == 0){             // 일주가 목오행
        var ss_o = ["비견","식신","편재","편관","편인"] // 오행이 같음
        var ss_x = ["비겁","상관","정재","정관","정인"] // 오행이 다름
      }
      else if (this.ganzi[4].value == 1){             // 일주가 화오행
        var ss_o = ["편인","비견","식신","편재","편관"] // 오행이 같음
        var ss_x = ["정인","비겁","상관","정재","정관"] // 오행이 다름
      }
      else if (this.ganzi[4].value == 2){             // 일주가 토오행
        var ss_o = ["편관","편인","비견","식신","편재"] // 오행이 같음
        var ss_x = ["정관","정인","비겁","상관","정재"] // 오행이 다름
      }
      else if (this.ganzi[4].value == 3){                  // 일주가 금오행
        var ss_o = ["편재","편관","편인","비견","식신"] // 오행이 같음
        var ss_x = ["정재","정관","정인","비겁","상관"] // 오행이 다름
      }
      else if (this.ganzi[4].value == 4){             // 일주가 수오행
        var ss_o = ["식신","편재","편관","편인","비견"] // 오행이 같음
        var ss_x = ["상관","정재","정관","정인","비겁"] // 오행이 다름
      }
      if (this.ganzi[4].umyang == b){
        return ss_o[a]
      }
      else {
        return ss_x[a]
      }
    },
    change_umyang(a){
      if (a == "양"){
        return "+"
      }
      else if(a == "음"){
        return "-"
      }
    },
    get_umyang(a){
      const sameChar1 = (element) => element == a;
      const start1 = this.only_Sky.findIndex(sameChar1)
      var aa = this.sky_char[start1].umyang
      var res = this.change_umyang(aa)
      return res
    },
    change_ganzi(){
      //천간 넣기
      var index = [4,0,2,6]
      for (let ii = 0;ii<4;ii++){
        var i = index[ii]
        if (i==4){
          for (let k =0; k<10;k++){
          if (this.typed_ganzi.split('')[i] == this.sky_char[k].id){
            this.ganzi[i].hanja = this.sky_char[k].hanja
            this.ganzi[i].value = this.sky_char[k].value
            this.ganzi[i].umyang = this.change_umyang(this.sky_char[k].umyang)
            var five_hang = this.ganzi[i].value
            this.ganzi[i].ten_shin = "견겁"
          }
          }  
        }
        else{
          for (let k =0; k<10;k++){
            if (this.typed_ganzi.split('')[i] == this.sky_char[k].id){
              this.ganzi[i].hanja = this.sky_char[k].hanja
              this.ganzi[i].value = this.sky_char[k].value
              this.ganzi[i].umyang = this.change_umyang(this.sky_char[k].umyang)
              var five_hang = this.ganzi[i].value
              this.ganzi[i].ten_shin = this.get_10shin(five_hang, this.ganzi[i].umyang)
            }
          }
        }
      }
      //지지 넣기
      for (let i = 1; i<12;i+=2){//1,3,5,7,9,11
        if ((i!==9)&& (i!==11)){ //월지를 id 9번에 한번더 넣기
          for (let k =0; k<12;k++){
            if (this.typed_ganzi.split('')[i] == this.earth_char[k].id){
              this.ganzi[i].hanja = this.earth_char[k].hanja
              this.ganzi[i].value = this.earth_char[k].value
              this.ganzi[i].amjang = this.earth_char[k].amjang

              var empty = []
              empty.push(this.ganzi[i].amjang[0])
              for (var mm = 1; mm< this.ganzi[i].amjang.length;mm++){
                var fh = this.get_5hang(this.ganzi[i].amjang[mm][0])
                var uy = this.get_umyang(this.ganzi[i].amjang[mm][0])
                var ts = this.get_10shin(fh,uy)
                empty.push([this.ganzi[i].amjang[mm][0],this.ganzi[i].amjang[mm][1],ts])  
              }
              this.ganzi[i].amjang = empty                   
            }
          }

        }
        else if(i==11){
          this.ganzi[i-2].hanja = this.ganzi[3].hanja
          this.ganzi[i-2].value = this.ganzi[3].value
          var empty = []
          empty.push(this.ganzi[3].amjang[0])
          for (var mm = 1; mm< this.ganzi[3].amjang.length;mm++){
            var fh = this.get_5hang(this.ganzi[3].amjang[mm][0])
            var uy = this.get_umyang(this.ganzi[3].amjang[mm][0])
            var ts = this.get_10shin(fh,uy)
            empty.push([this.ganzi[3].amjang[mm][0],this.ganzi[3].amjang[mm][1]*2,ts])  
            // empty.push([this.ganzi[3].amjang[mm][0],this.ganzi[3].amjang[mm][1],ts])  
          }
          this.ganzi[i-2].amjang = empty              
          }
        else {
          this.ganzi[i-1].hanja = this.ganzi[3].hanja
          this.ganzi[i-1].value = this.ganzi[3].value
          this.ganzi[i-1].amjang = this.ganzi[3].amjang
        }
      }
    },
    get_BigLuck(){
      const empty_array = new Array()
      let k;
      if ( this.ganzi[0].umyang !== this.gender){
        // 음양이 불일치할경우 역순
        const sameChar1 = (element) => element == this.ganzi[2].hanja;
        const sameChar2 = (element) => element == this.ganzi[3].hanja;

        const start1 = this.rev_Sky.findIndex(sameChar1)
        const start2 = this.rev_Earth.findIndex(sameChar2)

        k = 1
        for (let i = start2; i<start2+12; i++){
          empty_array.push( [[this.rev_Sky[(start1+k)%10],this.rev_Earth[(i+1)%12]],
            this.rev_Earth_amjang[(i+1)%12]])
          k ++
        }

        return empty_array
      }
      else if (this.ganzi[0].umyang == this.gender) {
        // 음양이 일치할경우 정순
        const sameChar1 = (element) => element == this.ganzi[2].hanja;
        const sameChar2 = (element) => element == this.ganzi[3].hanja;
        const start1 = this.only_Sky.findIndex(sameChar1)
        const start2 = this.only_Earth.findIndex(sameChar2)
        k = 1
        for (let i = start2; i<start2+12; i++){
          empty_array.push( [[this.only_Sky[(start1+k)%10], this.only_Earth[(i+1)%12]],
            this.only_Earth_amjang[(i+1)%12]] )
          k ++
        }
        return empty_array
      }
    },
    update_Bigluck(i){
      if (i==12){
        this.big_luck[0].big_luck_sky = 'o'
        this.big_luck[0].umyang = ''
        this.big_luck[0].ten_shin = ''
        this.big_luck[1].big_luck_ear = 'o'
        this.big_luck[2].big_luck_Amzang = [["o","o","o"],["o","o","o"],["o","o","o"],["o","o","o"]]
      }
      else{
        this.big_luck[0].big_luck_sky = this.get_BigLuck()[i][0][0]
        var five_hang = this.get_5hang(this.get_BigLuck()[i][0][0])
        this.big_luck[0].umyang = this.get_umyang(this.get_BigLuck()[i][0][0])
        this.big_luck[0].ten_shin = this.get_10shin(five_hang, this.get_umyang(this.get_BigLuck()[i][0][0]))
        this.big_luck[1].big_luck_ear = this.get_BigLuck()[i][0][1]

        var empty = []
        for (var mm=1; mm<this.get_BigLuck()[i][1].length;mm++){
          var fh = this.get_5hang(this.get_BigLuck()[i][1][mm][0])
          var uy = this.get_umyang(this.get_BigLuck()[i][1][mm][0])
          var ts = this.get_10shin(fh,uy)
          empty.push([this.get_BigLuck()[i][1][mm][0],this.get_BigLuck()[i][1][mm][1]*2,ts])
        }

        this.big_luck[2].big_luck_Amzang = empty
        var a_arr = this.get_BigLuck()[i][1].slice(1,this.get_BigLuck()[i][1].length)
        for (let k =0;k<a_arr.length;k++){
          var a5 = this.get_5hang(a_arr[k][0])
          this.gisae[a5].content.push( 
            [this.get_BigLuck()[i][0][1]+'中'+a_arr[k][0],
              a_arr[k][1]] )
          this.gisae[a5].content.push( 
            [this.get_BigLuck()[i][0][1]+'中'+a_arr[k][0],
              a_arr[k][1]] )
          this.gisae[a5].value += a_arr[k][1]*2
        }
      }
    },
    reset_gisae(){
      for (var i = 0; i<5;i++){
        this.gisae[i].value = 0
        this.gisae[i].content = []
      }
    },
    get_5hang(a){
      const sameChar1 = (element) => element == a
      const start1 = this.only_Sky.findIndex(sameChar1)
      return this.sky_char[start1].value
    },
    get_dup_gisae(ar){
      const result = []
      ar.forEach((x)=>{
        result[x] = (result[x] || 0)+1;
      })
      var tt = Object.entries(result)
      return tt
    },
    cal_gisae(index){
      this.reset_gisae()
      this.update_Bigluck(index)
      for (var i = 1; i<10; i+=2){
        if (i !== 9){
          var l =this.ganzi[i].amjang.length
          for (var k=1; k<l; k++){
            a = this.ganzi[i].amjang[k][0]
            a5 = this.get_5hang(a)
            this.gisae[a5].content.push( 
              [this.ganzi[i].hanja+'中'+this.ganzi[i].amjang[k][0],
                this.ganzi[i].amjang[k][1]] )
            this.gisae[a5].value += this.ganzi[i].amjang[k][1]
          }
        }
        else {
          var l =this.ganzi[i-1].amjang.length
          for (var k=1; k<l; k++){
            var a = this.ganzi[i-1].amjang[k][0]
            var a5 = this.get_5hang(a)
            this.gisae[a5].content.push( 
              [this.ganzi[i-1].hanja+'中'+this.ganzi[i-1].amjang[k][0],
                this.ganzi[i-1].amjang[k][1]] )
            this.gisae[a5].value += this.ganzi[i-1].amjang[k][1]
          }
        }
      }
      for (var i=0;i<5;i++){
        var dup = this.get_dup_gisae(this.gisae[i].content)
        this.gisae[i].content = []
        if (dup.length !== 0){
          for (var j=0; j<dup.length;j++){
            var a = dup[j][0].slice(4,dup[j][0].length)
            var nn = dup[j][1]
            this.gisae[i].content.push([dup[j][0].slice(0,3),nn*a])
          }    
        }
      }
    }                    
  },
  computed: {                 
    rev_Sky() {
      const empty_array = new Array()
      for (let i =(this.sky_char.length-1); i>-1;i--){
        empty_array.push( this.sky_char[i].hanja)
      }
      return empty_array
    }, 
    rev_Earth() {
      const empty_array1 = new Array()
      for (let i =(this.earth_char.length-1); i>-1;i--){
        empty_array1.push( this.earth_char[i].hanja)
      }
      return empty_array1
    },
    rev_Earth_amjang() {
      const empty_array1 = new Array()
      for (let i =(this.earth_char.length-1); i>-1;i--){
        empty_array1.push( this.earth_char[i].amjang)
      }
      return empty_array1
    },
    only_Sky() {
      const empty_array = new Array()
      for (let i =0; i< this.sky_char.length;i++){
        empty_array.push( this.sky_char[i].hanja)
      }
      return empty_array
    }, 
    only_Earth() {
      const empty_array = new Array()
      for (let i =0; i<this.earth_char.length; i++){
        empty_array.push( this.earth_char[i].hanja)
      }
      return empty_array
    },
    only_Earth_amjang() {
      const empty_array = new Array()
      for (let i =0; i<this.earth_char.length; i++){
        empty_array.push( this.earth_char[i].amjang)
      }
      return empty_array
    }
  }
}
</script>

<style>
div.wrap{
        width: 100%;
    }
    div.left {
        width: 30%;
        float: left;
    }
    div.center {
        width: 60%;
        float: left;
    }
    div.right{
      width: 10%;
      float: left;
    }
div.title{
  text-align: center
}
form.form2 {
    /* Just to center the form on the page */
    margin: 0 auto;
    width: 200px;
    /* To see the outline of the form */
    padding: 1em;
    border: 1px solid #CCC;
    border-radius: 1em;
}
div.v1 {
  font-size: 300%;
  writing-mode: vertical-rl;
}
div.single_letter{
  text-align: center;
  font-size: 200%;
  /* display: inline-block; */
  display: block;
  border: 1px solid black;
  width: 70px;
  height: 70px;
}
div.amjang_letter{
  text-align: center;
  font-size: 100%;
  border: 1px solid black;
  width: 70px;
  height: 25px;
  /* display: inline-block; */
  display: block;
}

span.amjang_letter_1{
  font-size: 80%;
}
span.amjang_letter_2{
  font-size: 20%;
}

div.single_pillar{  
  display: inline-block;
}
div.four_pillar {
  flex-wrap: nowrap;
  display: flex;
  padding-top: 50px;
  padding-left: 100px;
  width: 400px;
  height: 350px;
  background-color: rgb(150, 146, 146);
}

span.green{
  background-color: green;
}
span.red{
  background-color: red;
}
span.yellow{
  background-color: yellow;
}
span.white{
  background-color: white;
}
span.black{
  color: white;
  background-color: black;
}



div.pentagon {
  width: 300px;
  height: 300px;
  background-color: rgb(150, 146, 146);
}

.pentagon span:nth-child(1){
  position: relative;
  top: 50px;
  left: 135px;
  font-weight: bold;
}
.pentagon span:nth-child(6) {
  position: relative;
  font-weight: bold;
}
.pentagon span:nth-child(11) {
  font-weight: bold;
}
.pentagon span:nth-child(2) {
  position: relative;
  top: 90px;
  left: 170px;
  font-weight: bold;
}
.pentagon span:nth-child(7) {
  font-weight: bold;
}
.pentagon span:nth-child(12) {
  font-weight: bold;
}
.pentagon span:nth-child(3) {
  position: relative;
  top: 0px;
  left: 0px;
  font-weight: bold;
}
.pentagon span:nth-child(8) {
  font-weight: bold;
}
.pentagon span:nth-child(13) {
  font-weight: bold;
}
.pentagon span:nth-child(4) {
  font-weight: bold;
}
.pentagon span:nth-child(9) {
  font-weight: bold;
}
.pentagon span:nth-child(14) {

  font-weight: bold;
}
.pentagon span:nth-child(5) {
  font-weight: bold;
}
.pentagon span:nth-child(10) {

  font-weight: bold;
}
.pentagon span:nth-child(15) {
  font-weight: bold;
}
/* .pentagon span:nth-child(4) {
  position: absolute;
  top: 90px;
  left: 80px;
  width: 20px;
  height: 20px;
  padding: 5px;
  border-radius: 15px;
  background-color:white;
  border: 1px solid black;
}
.pentagon span:nth-child(5) {
  position: absolute;
  top: 50px;
  left: 60px;
} */
</style>
