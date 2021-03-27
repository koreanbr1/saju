<template>
  <div id="HelloWorld">
    성함
    <form>
      <input type="name" v-model="name"><br>
    </form>

    <br>
    성별(남자는 '양', 여자는 '음'으로 입력)
    <form>
      <input type="gender" v-model="gender"><br>
    </form>
    <br>

    干支 입력 (연간,연지...시간,시지 순으로)
    <br>test: 신미을미신사을미<br>
    test: 임신갑진임자경자<br>
    test:임자임자병술무술
    <form>
      <input
        type="text"
        placeholder="간지를 한글로 입력하세요"
        v-model="typed_ganzi" />
    </form>
    <br>
    <button @click="[change_ganzi(),cal_gisae(12),cal_5hang_cons()]">간지입력완료</button>
    <table >
      <tr>
        <th> 大運 </th>
        <th v-for = "i in ['時柱','日柱','月柱','年柱']" :key="i">{{ i }}</th>
      </tr>
      <tr>
        <th> {{big_luck[0].big_luck_sky}}<sup>{{big_luck[0].umyang}}</sup><sub>{{big_luck[0].ten_shin}}</sub> </th>
        <th v-for = "i in [6,4,2,0]" :key="i">{{ ganzi[i].hanja }}<sup>{{ganzi[i].umyang}}</sup><sub>{{ganzi[i].ten_shin}}</sub></th>
      </tr>
      <tr>
        <th> {{big_luck[1].big_luck_ear}} </th>
        <th v-for = "i in [7,5,9,1]" :key="i">{{ ganzi[i].hanja }}</th>
      </tr>
      <tr>
        <th> {{big_luck[2].big_luck_Amzang[0]}} </th>
        <th v-for = "i in [7,5,9,1]" :key="i">{{ ganzi[i].amjang[1] }}</th>
      </tr>
      <tr>
        <th> {{big_luck[2].big_luck_Amzang[1]}} </th>
        <th v-for = "i in [7,5,9,1]" :key="i">{{ ganzi[i].amjang[2] }}</th>
      </tr>
      <tr>
        <th> {{big_luck[2].big_luck_Amzang[2]}} </th>
        <th v-for = "i in [7,5,9,1]" :key="i">{{ ganzi[i].amjang[3] }}</th>
      </tr>
    </table>

    運路(운로)
    <td>
          <button @click=[update_Bigluck(12),cal_gisae(12)]>원국만 보기</button>
    </td>
    <table>
      <tr>    
        <td v-for = "i in [11,10,9,8,7,6,5,4,3,2,1,0]" :key="i">
          <button @click=[update_Bigluck(i),cal_gisae(i)]>{{ get_BigLuck()[i][0][0] }}<br>
            {{get_BigLuck()[i][0][1] }}
          </button>
          <select>
            <option disabled value="">吉凶</option>
            <option v-for= "option in GH" :key= "option.text">
            {{ option.text }}
            </option>
          </select>
        </td>
      </tr>
    </table>
    {{gisae[0].value}},{{gisae[0].content}}<br>
    {{gisae[1].value}},{{gisae[1].content}}<br>
    {{gisae[2].value}},{{gisae[2].content}}<br>
    {{gisae[3].value}},{{gisae[3].content}}<br>
    {{gisae[4].value}},{{gisae[4].content}}<br>
    <div class="pentagon" >
      <span v-for= "i in this.fhang_cons" :key=i style="border: 1px solid black; padding: 5px;">{{gisae[i].id }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      fhang_cons: [],
      typed_ganzi: '신미을미신사을미',
      ganzi: [
        {id: 0, hanja: "o", value: "o", umyang: "o", ten_shin: ""}, //연간 0
        {id: 1, hanja: "o", value: "o", amjang: [] },//연지 1
        {id: 2, hanja: "o", value: "o", umyang: "o", ten_shin: ""}, //월간 2
        {id: 3, hanja: "o", value: "o", amjang: [] },//월지 3
        {id: 4, hanja: "o", value: "o", umyang: "o", ten_shin: ""}, //일간 4
        {id: 5, hanja: "o", value: "o", amjang: [] },//일지 5
        {id: 6, hanja: "o", value: "o", umyang: "o", ten_shin: ""}, //시간 6
        {id: 7, hanja: "o", value: "o", amjang: [] },//시지 7
        {id: 9, hanja: "o", value: "o", amjang: [] }, //월지 한번더 8
        {id: 11, hanja: "o", value: "o", amjang: [] } //월지 한번더 9
      ],
      name: '장승표',
      gender: '양',
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
        {big_luck_sky: '', umyang: '', ten_shin: '', value: 0},
        {big_luck_ear: ''},
        {big_luck_Amzang: []},
      ],
      GH: [
        { text: '??'},
        { text: '吉' },
        { text: '平'},
        { text: '凶'},
      ]
    }
  },
  methods: {
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
        this.big_luck[0].big_luck_sky = ''
        this.big_luck[0].umyang = ''
        this.big_luck[0].ten_shin = ''
        this.big_luck[1].big_luck_ear = ''
        this.big_luck[2].big_luck_Amzang = ''
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
.pentagon {
  position: relative;
  width: 300px;
  height: 300px;
  background-color: gray;
}

.pentagon span:nth-child(1) {
  position: absolute;
  top: 50px;
  left: 130px;
  font-weight: bold;
  background-color: green;
}
.pentagon span:nth-child(2) {
  position: absolute;
  top: 110px;
  left: 200px;
  font-weight: bold;
  background-color: red;
}
.pentagon span:nth-child(3) {
  position: absolute;
  top: 190px;
  left: 160px;
  font-weight: bold;
  background-color: yellow;
}
.pentagon span:nth-child(4) {
  position: absolute;
  top: 190px;
  left: 70px;
  font-weight: bold;
  background-color:white;
}
.pentagon span:nth-child(5) {
  position: absolute;
  top: 110px;
  left: 50px;
  font-weight: bold;
  color: white;
  background-color:black;
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
