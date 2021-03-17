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
    <button @click="[change_ganzi(),cal_gisae(12)]">간지입력완료</button>
    <table>
      <tr>
        <th> 大運 </th>
        <th v-for ="i in ['時柱','日柱','月柱','年柱']" :key="i">{{ i }}</th>
      </tr>
      <tr>
        <th> {{big_luck_sky}} </th>
        <th v-for ="i in [6,4,2,0]" :key="i">{{ ganzi[i].hanja }}</th>
      </tr>
      <tr>
        <th> {{big_luck_ear}} </th>
        <th v-for ="i in [7,5,3,1]" :key="i">{{ ganzi[i].hanja }}</th>
      </tr>
      <tr>
        <th> {{big_luck_Amzang}} </th>
        <th v-for = "i in [7,5,3,1]" :key="i">{{ ganzi[i].amjang[0] }}</th>
      </tr>
    </table>

    運路(운로)
    <br>
    <table>
      <tr>
        <td>
          <button @click=[update_Bigluck(12),cal_gisae(12)]>원국만 보기</button>
        </td>
        <td v-for ="i in [11,10,9,8,7,6,5,4,3,2,1,0]" :key="i">
          <button @click=[update_Bigluck(i),cal_gisae(i)]>{{ get_BigLuck()[i][0][0] }}<br>
            {{get_BigLuck()[i][0][1] }}
          </button>
          <br>
          <select>
            <option disabled value="">吉凶</option>
            <option v-for="option in options" :key="option">
            {{ option.text }}
            </option>
          </select>
        </td>
      </tr>
    </table>
    <div class="pentagon">
      <span v-for="i in [0,1,2,3,4]" :key="i" :class="{red: true}">{{gisae[i].id }},{{gisae[i].content}}</span>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      typed_ganzi: '신미을미신사을미',
      ganzi: [
        {id: 0, hanja: "", value: "", umyang: ""}, //연간
        {id: 1, hanja: "", value: "", amjang: [] },//연지
        {id: 2, hanja: "", value: "", umyang: ""}, //월간
        {id: 3, hanja: "", value: "", amjang: [] },//월지
        {id: 4, hanja: "", value: "", umyang: ""}, //일간
        {id: 5, hanja: "", value: "", amjang: [] },//일지
        {id: 6, hanja: "", value: "", umyang: ""}, //시간
        {id: 7, hanja: "", value: "", amjang: [] },//시지
        {id: 9, hanja: "", value: "", amjang: [] } //월지 한번더
      ],
      name: '',
      gender: '양',
      gisae:[
        {id: '木', value: 0, content: []},
        {id: '火', value: 0, content: []},
        {id: '土', value: 0, content: []},
        {id: '金', value: 0, content: []},
        {id: '水', value: 0, content: []},
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
          '乙甲', ['乙',30]]},
        {id: '진', hanja: "辰", value: 2, amjang: [
          '戊癸乙',['戊',18],['癸',3],['乙',9]]},
        {id: '사', hanja: "巳", value: 1, amjang: [
          '丙庚戊',['丙',16],['庚',7],['戊',7]]},
        {id: '오', hanja: "午", value: 1, amjang: [
          '丁己丙',['丁',30],]},
        {id: '미', hanja: "未", value: 2, amjang: [
          '己乙丁',['己',18],['乙',3],['丁',9]]},
        {id: '신', hanja: "申", value: 3, amjang: [
          '庚壬戊',['庚',16],['壬',7],['戊',7]]},
        {id: '유', hanja: "酉", value: 3, amjang: [
          '辛庚',['辛',30]]},
        {id: '술', hanja: "戌", value: 2, amjang: [
          '戊丁辛',['戊',18],['丁',3],['辛',9]]},
        {id: '해', hanja: "亥", value: 4, amjang: [
          '壬甲戊',['壬',16],['甲',7],['戊',7]]},
        {id: '자', hanja: "子", value: 4, amjang: [
          '癸壬',['癸',30]]},
        {id: '축', hanja: "丑", value: 2, amjang: [
          '己辛癸',['己',18],['辛',3],['癸',9]]},
      ],
      big_luck_sky: 'O',
      big_luck_ear: 'O',
      big_luck_Amzang: 'O',
      options: [
        { text: '○' },
        { text: '△'},
        { text: 'Χ'},
        { text: '??'}
      ]
    }
  },
  methods: {
    change_ganzi(){
      //천간 넣기
      for (let i = 0; i<9;i+=2){
        for (let k =0; k<10;k++){
          if (this.typed_ganzi.split('')[i] == this.sky_char[k].id){
            this.ganzi[i] = this.sky_char[k]
          }
        }
      }
      //지지 넣기
      for (let i = 1; i<10;i+=2){
        if (i!==9){ //월지를 id 9번에 한번더 넣기
          for (let k =0; k<12;k++){
            if (this.typed_ganzi.split('')[i] == this.earth_char[k].id){
              this.ganzi[i].hanja = this.earth_char[k].hanja
              this.ganzi[i].value = this.earth_char[k].value
              this.ganzi[i].amjang = this.earth_char[k].amjang
            }
          }

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
          this.big_luck_sky = 'O'
          this.big_luck_ear = 'O'
          this.big_luck_Amzang = 'O'
        }
        else{
          this.big_luck_sky = this.get_BigLuck()[i][0][0]
          this.big_luck_ear = this.get_BigLuck()[i][0][1]
          this.big_luck_Amzang = this.get_BigLuck()[i][1][0]
          a_arr = this.get_BigLuck()[i][1].slice(1,this.get_BigLuck()[i][1].length)
          for (let k =0;k<a_arr.length;k++){
            a5 = this.get_5hang(a_arr[k][0])
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
        for (i = 0; i<5;i++){
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
        tt = Object.entries(result)
        return tt
      },
    cal_gisae(index){
      this.reset_gisae()
      this.update_Bigluck(index)
      for (let i = 1; i<10; i+=2){
        if (i !== 9){
          l =this.ganzi[i].amjang.length
          for (k=1; k<l; k++){
            a = this.ganzi[i].amjang[k][0]
            a5 = this.get_5hang(a)
            this.gisae[a5].content.push( 
              [this.ganzi[i].hanja+'中'+this.ganzi[i].amjang[k][0],
                this.ganzi[i].amjang[k][1]] )
            this.gisae[a5].value += this.ganzi[i].amjang[k][1]
          }
        }
        else {
          l =this.ganzi[i-1].amjang.length
          for (k=1; k<l; k++){
            a = this.ganzi[i-1].amjang[k][0]
            a5 = this.get_5hang(a)
            this.gisae[a5].content.push( 
              [this.ganzi[i-1].hanja+'中'+this.ganzi[i-1].amjang[k][0],
                this.ganzi[i-1].amjang[k][1]] )
            this.gisae[a5].value += this.ganzi[i-1].amjang[k][1]
          }
        }
      }
      for (let i=0;i<5;i++){
        dup = this.get_dup_gisae(this.gisae[i].content)
        this.gisae[i].content = []
        if (dup.length !== 0){
          for (j=0; j<dup.length;j++){
            a = dup[j][0].slice(4,dup[j][0].length)
            n = dup[j][1]
            this.gisae[i].content.push([dup[j][0].slice(0,3),n*a])
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
  width: 200px;
  height: 200px;
  background-color: gray;
}

.pentagon span:nth-child(1) {
  position: absolute;
  top: 10px;
  left: 100px;
  color: red;
}
.pentagon span:nth-child(2) {
  position: absolute;
  top: 50px;
  left: 130px;
  background-color: green;
}
.pentagon span:nth-child(3) {
  position: absolute;
  top: 90px;
  left: 110px;
  font-weight: bold;
}
.pentagon span:nth-child(4) {
  position: absolute;
  top: 90px;
  left: 80px;
  width: 20px;
  height: 20px;
  padding: 5px;
  border-radius: 15px;
  border: 1px solid black;
}
.pentagon span:nth-child(5) {
  position: absolute;
  top: 50px;
  left: 60px;
}
</style>
