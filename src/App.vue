<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <Table></Table>
    <el-button @click="insertRecord">插入测试</el-button> -->
    <div class="container"> 
      <!-- 定义页眉 -->     
        <header>
          <h1>Memory Management <br>请求调页存储管理模拟</h1>
          <h3 class="subTitle">Tongji SSE</h3>
          <h3 class="subTitle">1950679 孟繁青</h3>
        </header>

     <!--<main id="main">--> 
       <!--算法选择-->
      <p>
        <section class="box01">
          <h3>请选择页面置换算法</h3><br>
        <el-select v-model="value" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
            <br>
            <br>
        </section>
      </p>

        <!--项目基础信息展示-->
      <p>
        <section class="box">
          <h3>参数信息</h3><br>
          <p>内存块数：<span id="overall_memory_blocks">4</span></p><br>
          <p>总指令数：<span id="overall_instructions">320</span></p><br>
          <p>每页存放指令数：<span id="instruction_per_page">10</span></p>
        </section>

        <!--结果统计分析-->
        <section class="box">
          <h3>结果统计分析</h3>
          <p><strong>FIFO</strong></p>
          <p>缺页次数：<span id="FIFO_miss_page_count">{{FIFOnumber}}</span></P>
          <P>缺页率：<span id="FIFO_miss_page_rate">{{FIFOpercent}}</span></p>
          <p><strong>LRU</strong></p>
          <p>缺页次数：<span id="LRU_miss_page_count">{{LRUnumber}}</span></P>
          <P>缺页率：<span id="LRU_miss_page_rate">{{LRUpercent}}</span></p>
        </section>
        <section class="box">
          <h3>结果统计分析</h3>
          <p><strong>OPT</strong></p>
          <p>缺页次数：<span id="FIFO_miss_page_count">{{OPTnumber}}</span></P>
          <P>缺页率：<span id="FIFO_miss_page_rate">{{OPTpercent}}</span></p>
          <p><strong>LFU</strong></p>
          <p>缺页次数：<span id="LRU_miss_page_count">{{LFUnumber}}</span></P>
          <P>缺页率：<span id="LRU_miss_page_rate">{{LFUpercent}}</span></p>
        </section>
        <section class="box">
          <h3>结果统计分析</h3>
          <p><strong>CLOCK</strong></p>
          <p>缺页次数：<span id="FIFO_miss_page_count">{{CLOCKnumber}}</span></P>
          <P>缺页率：<span id="FIFO_miss_page_rate">{{CLOCKpercent}}</span></p>
          <br>
          <p>下面的init是按项目要求初始化指令，init2是完全随机初始化指令</p>
        </section>
      </p>

        <!-- <p><button id="start-Btn" @click="start()"><strong>START</strong> </button></p><br><br><br><br><br> -->
        <el-button type="primary" plain id="start-Btn" @click="generate()"><strong>INIT</strong></el-button>
        <br>
        <br>
        <el-button type="primary" plain id="start-Btn2" @click="generate()"><strong>INIT2</strong></el-button>
        <br>
        <br>
        <el-button type="primary" plain id="start-Btn" @click="start()" width= "100px"><strong>START</strong></el-button>
        <!--结果展示区域-->
        <div class="show">
    <el-table
    :data="tableData"
    style="width: 100%"
    max-height="700"
    class = "myTable">
    <el-table-column
      fixed
      prop="num"
      label="Sequence"
      width="120">
    </el-table-column>
    <el-table-column
      prop="Instruction"
      label="Instruction"
      width="120">
    </el-table-column>
    <el-table-column
      prop="Frame1"
      label="Frame1"
      width="120">
    </el-table-column>
    <el-table-column
      prop="Frame2"
      label="Frame2"
      width="120">
    </el-table-column>
    <el-table-column
      prop="Frame3"
      label="Frame3"
      width="120">
    </el-table-column>
    <el-table-column
      prop="Frame4"
      label="Frame4"
      width="120">
    </el-table-column>
    <el-table-column
    prop="Discription"
    label="Discription"
    width="150">
  </el-table-column>
  </el-table>
        </div>

        <div class="footer">
          <pre>
            SSE Tongji University

            Copyright @ 2021 FanqingM All Rights Reserved.
          </pre>
        </div>
    </div>
  </div>
</template>

<script>
// import Table from './components/Table.vue'

export default {
  name: 'App',
  components: {
    // Table
  },
  methods: {
      deleteRow(index, rows) {
        rows.splice(index, 1);
      },
      searchLFU(currentCnt)
      {
        var res=[0,0,0,0];
        var temp1 = this.memory[0];
        var temp2 = this.memory[1];
        var temp3 = this.memory[2];
        var temp4 = this.memory[3];     //记下当前4个内存块中的页号
        for(var i=currentCnt-1;i>=0;i--)
        {
          if(this.pageNumPath[i] == temp1)
          {
            res[0] = res[0]+1;
          }
          else if(this.pageNumPath[i] == temp2)
          {
            res[1] = res[1]+1;
          }
          else if(this.pageNumPath[i] == temp3)
          {
            res[2] = res[2]+1;
          }
          else if(this.pageNumPath[i] == temp4)
          {
            res[3] = res[3]+1;
          }
        }
        //下面找res数组中最小的对应的下表
        var imin = 0;
        var min = res[0];
        for(var n=1;n<4;n++)
        {
          if(res[n]<min)
          {
            min = res[n];
            imin = n;
          }
        }
        return imin;
      },
      searchOPT(currentCnt)    //指出当前的指令条数，从此往前找
      {
        //我们使用pageNumPath记录指令执行序列对应的页号的序列
        var res = [-1,-1,-1,-1];
        //此时内存块中都是有东西的，所以不可能出现-1
        var temp1 = this.memory[0];
        var temp2 = this.memory[1];
        var temp3 = this.memory[2];
        var temp4 = this.memory[3];     //记下当前4个内存块中的页号
        var i;
        var j;
        var k;
        var m;
        for(i = currentCnt+1;i<320;i++)
        {
          //一定出现过，不然就不会在内存快中存在了
          if(this.pageNumPath[i] == temp1)
          {
            res[0] = i;
            break;
          }
        }
        if(i==320)
        {
          res[0] = 320;
        }
        for(j = currentCnt+1;j<320;j++)
        {
          //一定出现过，不然就不会在内存快中存在了
          if(this.pageNumPath[j] == temp2)
          {
            res[1] = j;
            break;
          }
        }
        if(j==320)
        {
          res[1] = 320;
        }
        for(k = currentCnt+1;k<320;k++)
        {
          //一定出现过，不然就不会在内存快中存在了
          if(this.pageNumPath[k] == temp3)
          {
            res[2] = k;
            break;
          }
        }
        if(k==320)
        {
          res[2] = 320;
        }
        for(m = currentCnt+1;m<320;m++)
        {
          //一定出现过，不然就不会在内存快中存在了
          if(this.pageNumPath[m] == temp4)
          {
            res[3] = m;
            break;
          }
        }
        if(m==320)
        {
          res[3] = 320;
        }
        //下面找res数组中最小值对应的下表
        var imax = 0;
        var max = res[0];
        for(var n=1;n<4;n++)
        {
          if(res[n]>max)
          {
            max = res[n];
            imax = n;
          }
        }
        return imax;
      },
      searchLRU(currentCnt)    //指出当前的指令条数，从此往前找
      {
        //我们使用pageNumPath记录指令执行序列对应的页号的序列
        var res = [-1,-1,-1,-1];
        //此时内存块中都是有东西的，所以不可能出现-1
        var temp1 = this.memory[0];
        var temp2 = this.memory[1];
        var temp3 = this.memory[2];
        var temp4 = this.memory[3];     //记下当前4个内存块中的页号
        for(var i = currentCnt-1;i>=0;i--)
        {
          //一定出现过，不然就不会在内存快中存在了
          if(this.pageNumPath[i] == temp1)
          {
            res[0] = i;
            break;
          }
        }
        for(var j = currentCnt-1;j>=0;j--)
        {
          //一定出现过，不然就不会在内存快中存在了
          if(this.pageNumPath[j] == temp2)
          {
            res[1] = j;
            break;
          }
        }
        for(var k = currentCnt-1;k>=0;k--)
        {
          //一定出现过，不然就不会在内存快中存在了
          if(this.pageNumPath[k] == temp3)
          {
            res[2] = k;
            break;
          }
        }
        for(var m = currentCnt-1;m>=0;m--)
        {
          //一定出现过，不然就不会在内存快中存在了
          if(this.pageNumPath[m] == temp4)
          {
            res[3] = m;
            break;
          }
        }
        //下面找res数组中最小值对应的下表
        var imin = 0;
        var min = res[0];
        for(var n=1;n<4;n++)
        {
          if(res[n]<min)
          {
            min = res[n];
            imin = n;
          }
        }
        return imin;
      },
      searchFIFO()
      {
        var imin = 0;
        var min = this.memoryFIFO[0];
        for(var i=1;i<4;i++)
        {
          if(this.memoryFIFO[i]<min)
          {
            min = this.memoryFIFO[i];
            imin = i;
          }
        }
        return imin;
      },
      whoIsEmpty()
      {
        var i = 0;
        for(i=0;i<4;i++)
        {
          if(this.memory[i] == -1)
          {
            console.log("第"+(i+1)+"个内存块目前空闲");
            return i;
          }
        }
        if(i == 4)
        {
          //console.log("没有内存块空闲需要进行置换算法");
          return -1;
        }
      },
      convertToPageNum(ins)
      {
        return Math.floor(ins / 10);
      },
      generate()    //初始化指令序列
      { 
        this.isInit = true;
        var currentCount = 0;
        var flag = true;
        var temp = Math.floor(Math.random() * 320);   //0--319
        this.instructionsPath.push(temp);
        this.pageNumPath.push(this.convertToPageNum(temp));
        this.instructionsPath.push(temp+1);
        this.pageNumPath.push(this.convertToPageNum(temp+1));
        currentCount = 2;
        while(flag)
        {
          if(currentCount >= 320)
          {
            break;
          }
          var temp2 = Math.floor(Math.random() * (temp));  //0--temp1-1
          this.instructionsPath.push(temp2);
          this.pageNumPath.push(this.convertToPageNum(temp2));
          this.instructionsPath.push(temp2+1);
          this.pageNumPath.push(this.convertToPageNum(temp2+1));
          currentCount = currentCount+2;
          if(currentCount >= 320)
          {
            break;
          }
          var temp3 = Math.floor(Math.random() * (320-temp2-2))+(temp2+2);
          this.instructionsPath.push(temp3);
          this.pageNumPath.push(this.convertToPageNum(temp3));
          this.instructionsPath.push(temp3+1);
          this.pageNumPath.push(this.convertToPageNum(temp3+1));
          currentCount = currentCount+2;
          if(currentCount >= 320)
          {
            break;
          }
          temp = temp3;
        }
      },
      generate2()
      {
        this.isInit = true;
        for(var i=0;i<320;i++)
        {
          this.instructionsPath[i] = Math.floor(Math.random()*(320));
          this.pageNumPath[i] = this.convertToPageNum(this.instructionsPath[i]);
        }
      },
      isInMemory(ins)     //ins是指令的编号0-319，判断指令是否已经在内存中（即不需要发生缺页中断）
      {
        for (let i = 0; i < 4; i++) {
            var j = Math.floor(ins / 10)//该指令所在页号
            if (j === this.memory[i]) {
                //console.log("在第" + this.memory[i] + "页")
                return i //找到，找到就不需要缺页中断，直接运行下一条指令
            }
        }
        return -1 //返回页号，要将该页掉入内存的某一个块中，发生了缺页中断
      },
      CLOCK()
      {
        var pointer = 0;
        for(var i=0;i<320;i++)
        {
          var flag = this.isInMemory(this.instructionsPath[i])
          if(flag >= 0)       //说明已经在某个内存快中了
          {
            console.log("指令"+this.instructionsPath[i]+"已经在内存快"+(flag+1)+"中");
            this.memoryUseBit[flag] = 1;
            this.tableData.push({
            num: i+1,
            Instruction: this.instructionsPath[i],
            Frame1: this.memory[0],
            Frame2: this.memory[1],
            Frame3: this.memory[2],
            Frame4: this.memory[3],
            Discription:"指令"+this.instructionsPath[i]+"已经在内存块"+(flag+1)+"中"
            });
          }
          else
          {
            //说明发生了缺页中断
            this.missPageCLOCK = this.missPageCLOCK+1;
            console.log("发生了缺页中断");
            //第flag个页没有（从0计数）
            //先找有没有空块
            var flag2 = this.whoIsEmpty();
            if(flag2 == -1)
            {
              //说明需要进行置换
              while(this.memoryUseBit[pointer] === 1)
              {
                this.memoryUseBit[pointer] = 0;
                pointer = pointer+1;
                pointer = pointer%4;
              }
              this.memory[pointer] = this.convertToPageNum(this.instructionsPath[i]);
              this.memoryUseBit[pointer] = 1;
              //this.memoryFIFO[aim] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(pointer+1)+"个内存块中");
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"发生置换，指令"+this.instructionsPath[i]+"掉入第"+(pointer+1)+"个内存块中",
              });
            }
            else               //直接将该指令序号插入下表是flag2(此时是空的)的块
            {
              this.memoryUseBit[pointer] = 1;
              this.memory[pointer] = this.convertToPageNum(this.instructionsPath[i]); 
              pointer = pointer+1;
              pointer = pointer%4;
              //this.memory[flag2] = this.convertToPageNum(this.instructionsPath[i]);   //把这个指令所在页的页号扔进去
              //this.memoryFIFO[flag2] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(pointer+1)+"个内存块中"); 
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"仅发生缺页，指令"+this.instructionsPath[i]+"掉入第"+(pointer+1)+"个内存块中",
              });
            }
          }
        }
        this.CLOCKnumber=String(this.missPageCLOCK);
        this.CLOCKpercent=String(this.missPageCLOCK/320);
      },
      LFU()
      {
        //基本思想是过去访问的次数多，将来也会多。
        for(var i=0;i<320;i++)
        {
          var flag = this.isInMemory(this.instructionsPath[i])
          if(flag >= 0)       //说明已经在某个内存快中了
          {
            console.log("指令"+this.instructionsPath[i]+"已经在内存快"+(flag+1)+"中");
            this.tableData.push({
            num: i+1,
            Instruction: this.instructionsPath[i],
            Frame1: this.memory[0],
            Frame2: this.memory[1],
            Frame3: this.memory[2],
            Frame4: this.memory[3],
            Discription:"指令"+this.instructionsPath[i]+"已经在内存块"+(flag+1)+"中"
            });
          }
          else
          {
            //说明发生了缺页中断
            console.log("发生了缺页中断");
            //第flag个页没有（从0计数）
            this.missPageLFU = this.missPageLFU+1;
            //先找有没有空块
            var flag2 = this.whoIsEmpty();
            if(flag2 == -1)
            {
              //说明需要进行置换
              var aim = this.searchLFU(i);
              this.memory[aim] = this.convertToPageNum(this.instructionsPath[i]);
              //this.memoryFIFO[aim] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(aim+1)+"个内存块中");
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"发生置换，指令"+this.instructionsPath[i]+"掉入第"+(aim+1)+"个内存块中",
              });
            }
            else               //直接将该指令序号插入下表是flag2的块
            {
              this.memory[flag2] = this.convertToPageNum(this.instructionsPath[i]);   //把这个指令所在页的页号扔进去
              //this.memoryFIFO[flag2] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(flag2+1)+"个内存块中"); 
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"仅发生缺页，指令"+this.instructionsPath[i]+"掉入第"+(flag2+1)+"个内存块中",
              });
            }
          }
        }
        this.LFUnumber=String(this.missPageLFU);
        this.LFUpercent=String(this.missPageLFU/320);
      },
      OPT()
      {
        for(var i=0;i<320;i++)
        {
          var flag = this.isInMemory(this.instructionsPath[i])
          if(flag >= 0)       //说明已经在某个内存快中了
          {
            console.log("指令"+this.instructionsPath[i]+"已经在内存快"+(flag+1)+"中");
            this.tableData.push({
            num: i+1,
            Instruction: this.instructionsPath[i],
            Frame1: this.memory[0],
            Frame2: this.memory[1],
            Frame3: this.memory[2],
            Frame4: this.memory[3],
            Discription:"指令"+this.instructionsPath[i]+"已经在内存块"+(flag+1)+"中"
            });
          }
          else
          {
            //说明发生了缺页中断
            console.log("发生了缺页中断");
            //第flag个页没有（从0计数）
            this.missPageOPT = this.missPageOPT+1;
            //先找有没有空块
            var flag2 = this.whoIsEmpty();
            if(flag2 == -1)
            {
              //说明需要进行置换,我们需要查看此时4个内存块中的页号，谁是最早出现的，就换到这个内存块里面
              //我们找当前4个内存块中谁第一个有数据,事实上memoryFIFO中谁小谁就是先来的，要换掉他
              var aim = this.searchOPT(i);
              this.memory[aim] = this.convertToPageNum(this.instructionsPath[i]);
              //this.memoryFIFO[aim] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(aim+1)+"个内存块中");
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"发生置换，指令"+this.instructionsPath[i]+"掉入第"+(aim+1)+"个内存块中",
              });
            }
            else               //直接将该指令序号插入下表是flag2的块
            {
              this.memory[flag2] = this.convertToPageNum(this.instructionsPath[i]);   //把这个指令所在页的页号扔进去
              //this.memoryFIFO[flag2] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(flag2+1)+"个内存块中"); 
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"仅发生缺页，指令"+this.instructionsPath[i]+"掉入第"+(flag2+1)+"个内存块中",
              });
            }
          }
        }
        this.OPTnumber=String(this.missPageOPT);
        this.OPTpercent=String(this.missPageOPT/320);
      },
      LRU()
      {
        for(var i=0;i<320;i++)
        {
          var flag = this.isInMemory(this.instructionsPath[i])
          if(flag >= 0)       //说明已经在某个内存快中了
          {
            console.log("指令"+this.instructionsPath[i]+"已经在内存快"+(flag+1)+"中");
            this.tableData.push({
            num: i+1,
            Instruction: this.instructionsPath[i],
            Frame1: this.memory[0],
            Frame2: this.memory[1],
            Frame3: this.memory[2],
            Frame4: this.memory[3],
            Discription:"指令"+this.instructionsPath[i]+"已经在内存块"+(flag+1)+"中"
            });
          }
          else
          {
            //说明发生了缺页中断
            console.log("发生了缺页中断");
            //第flag个页没有（从0计数）
            this.missPageLRU = this.missPageLRU+1;
            //先找有没有空块
            var flag2 = this.whoIsEmpty();
            if(flag2 == -1)
            {
              //说明需要进行置换,我们需要查看此时4个内存块中的页号，谁是最早出现的，就换到这个内存块里面
              //我们找当前4个内存块中谁第一个有数据,事实上memoryFIFO中谁小谁就是先来的，要换掉他
              var aim = this.searchLRU(i);
              this.memory[aim] = this.convertToPageNum(this.instructionsPath[i]);
              //this.memoryFIFO[aim] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(aim+1)+"个内存块中");
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"发生置换，指令"+this.instructionsPath[i]+"掉入第"+(aim+1)+"个内存块中",
              });
            }
            else               //直接将该指令序号插入下表是flag2的块
            {
              this.memory[flag2] = this.convertToPageNum(this.instructionsPath[i]);   //把这个指令所在页的页号扔进去
              //this.memoryFIFO[flag2] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(flag2+1)+"个内存块中"); 
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"仅发生缺页，指令"+this.instructionsPath[i]+"掉入第"+(flag2+1)+"个内存块中",
              });
            }
          }
        }
        this.LRUnumber=String(this.missPageLRU);
        this.LRUpercent=String(this.missPageLRU/320);
      },
      FIFO()
      {
        for(var i=0;i<320;i++)
        {
          //内存快里面是页号，不是指令序号
          var flag = this.isInMemory(this.instructionsPath[i])
          if(flag >= 0)
          {
            console.log("指令"+this.instructionsPath[i]+"已经在内存快"+(flag+1)+"中");
            this.tableData.push({
            num: i+1,
            Instruction: this.instructionsPath[i],
            Frame1: this.memory[0],
            Frame2: this.memory[1],
            Frame3: this.memory[2],
            Frame4: this.memory[3],
            Discription:"指令"+this.instructionsPath[i]+"已经在内存块"+(flag+1)+"中"
            });
          }
          else    //已经发生了缺页，下面判断是否需要置换
          {
            console.log("发生了缺页中断");
            //第flag个页没有（从0计数）
            this.missPageFIFO = this.missPageFIFO+1;
            //先找有没有空块
            var flag2 = this.whoIsEmpty();
            if(flag2 == -1)    //说明需要进行置换
            {
              //我们找当前4个内存块中谁第一个有数据,事实上memoryFIFO中谁小谁就是先来的，要换掉他
              var aim = this.searchFIFO();
              this.memory[aim] = this.convertToPageNum(this.instructionsPath[i]);
              this.memoryFIFO[aim] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(aim+1)+"个内存块中");
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"发生置换，指令"+this.instructionsPath[i]+"掉入第"+(aim+1)+"个内存块中",
              });
            }
            else               //直接将该指令序号插入下表是flag2的块
            {
              this.memory[flag2] = this.convertToPageNum(this.instructionsPath[i]);   //把这个指令所在页的页号扔进去
              this.memoryFIFO[flag2] = i;  
              console.log("指令"+this.instructionsPath[i]+"掉入第"+(flag2+1)+"个内存块中"); 
              this.tableData.push({
              num: i+1,
              Instruction: this.instructionsPath[i],
              Frame1: this.memory[0],
              Frame2: this.memory[1],
              Frame3: this.memory[2],
              Frame4: this.memory[3],
              Discription:"仅发生缺页，指令"+this.instructionsPath[i]+"掉入第"+(flag2+1)+"个内存块中",
              });
            }
          }
        }
        this.FIFOnumber=String(this.missPageFIFO);
        this.FIFOpercent=String(this.missPageFIFO/320);
        
      },
      start()
      {
        if(this.isInit === false)
        {
          alert("🆘请先随机初始化指令，点击INIT按钮");
        }
        else
        {
          if(this.value === '')
          {
            alert("🆘请先选择一种方法");
          }
          else
          {
            this.tableData = [{}]
            //this.generate();
            for(var i=0;i<4;i++)
            {
              this.memory[i] = -1;
              this.memoryUseBit[i] = -1;
              this.memoryFIFO[i] = -1;
            }
            this.instructionsCurrentNum = 0;     
            if(this.value == 1)
            {
              this.missPageFIFO = 0;
              this.FIFOnumber = 'NULL';
              this.FIFOpercent = 'NULL';
              this.FIFO();
            }
            else if(this.value == 2)
            {
              this.missPageLRU = 0;
              this.LRUnumber = 'NULL';
              this.LRUpercent = 'NULL';
              this.LRU();
            }
            else if(this.value == 3)
            {
              this.missPageOPT = 0;
              this.OPTnumber = 'NULL';
              this.OPTpercent = 'NULL';
              this.OPT();
            }
            else if(this.value == 4)
            {
              this.missPageLFU = 0;
              this.LFUnumber = 'NULL';
              this.LFUpercent = 'NULL';
              this.LFU();
            }
            else if(this.value == 5)
            {
              this.missPageCLOCK = 0;
              this.CLOCKnumber = 'NULL';
              this.CLOCKpercent = 'NULL';
              this.CLOCK();
            }
          }
        }
      }
    },
    data() {
      return {
        options: [{
          value: 1,
          label: 'FIFO'
        }, {
          value: 2,
          label: 'LRU'
        }, {
          value: 3,
          label: 'OPT'
        }, {
          value: 4,
          label: 'LFU'
        },
        {
          value: 5,
          label: 'CLOCK'
        }],
        value: '',

        isInit: false,

        memoryBlock: 4,                //内存块数
        instructionsNum: 320,          //总指令数
        instructionsPerPage: 10,       //每页存放指令数
        pagesNum: 32,                  //总页数
        instructionsCurrentNum: 0,     //记录执行的指令个数
        missPageFIFO: 0,               //FIFO缺页个数
        missPageLRU: 0,                //LRU缺页个数
        missPageOPT: 0,                //OPT缺页个数
        missPageLFU: 0,                //LFU缺页个数
        missPageCLOCK: 0,              //CLOCK缺页个数
        memory: [...Array(4)].map(() => -1),           //4个内存中存储的页号
        memoryUseBit: [...Array(4)].map(() => -1),     //4个内存中当前页号的use bit，为了CLOCK算法
        memoryFIFO: [...Array(4)].map(()=> -1),        //记录FIFO中内存快被指令填满的先后顺序
        instructionsPath:[],   //指令执行序列
        pageNumPath:[],        //指令执行序列对应的页号的序列，为了LRU
        FIFOnumber:'NULL',
        FIFOpercent:'NULL',
        LRUnumber:'NULL',
        LRUpercent:'NULL',

        OPTnumber:'NULL',
        OPTpercent:'NULL',
        LFUnumber:'NULL',
        LFUpercent:'NULL',
        CLOCKnumber:'NULL',
        CLOCKpercent:'NULL',
        tableData: [{
        }]
      }
    }
}
</script>

<style>
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background: /*#E0F7FA*//*#E2F0D9*//*#ECF5E7*/#CCEDFF;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  font-size: 11px;
  color: /*#00838f*/#1178B7;
}
.myTable {
  margin-top: 100px;
}
#start-Btn {
  margin-top:30px;
}
.container {
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  width: 60%;
}


.box01{
  background: #fff;
  border-radius: 2px;
  display: inline-block;
  height: 150px; 
  margin: 2rem;
  position: relative;
  width: 400px;
  box-shadow: 0 19px 38px rgba(0, 0, 0, 0.3), 0 15px 12px rgba(0, 0, 0, 0.22);
}

.box{
  background: #fff;
  border-radius: 2px;
  display: inline-block;
  height: 240px;
  margin: 2rem;
  position: relative;
  width: 250px;
  box-shadow: 0 19px 38px rgba(0, 0, 0, 0.3), 0 15px 12px rgba(0, 0, 0, 0.22);
}

table,
th,
td {
    border: 1px solid #00838F;
    border-collapse: separate;
    
}

table {
  width: 80%;
  margin: auto;
  /*height: 100px;*/
}

th,
td {
    padding: 10px;
    width: 150px;
}

.show{
  height:650px;
  overflow-y: scroll;
  overflow-x: none;
}

#memory_table{
  width: 100%;
  margin: auto;
  /*height: 650px;*/
  vertical-align: middle;
}

.footer{
  display: inline-block;
  vertical-align: middle;
}


button{
  top: 12px;
  text-align: center;
  font-family: "Lucida Sans","Microsoft JhengHei";
  font-weight:300;
  position: relative;
  height: 40px;
  width: 80px;
  border: 1px solid #fff;
  background: /*#80CBC4*/#FDFCD6;
  border-radius: 6px;
  box-shadow: 0 19px 38px rgba(0, 0, 0, 0.3), 0 15px 12px rgba(0, 0, 0, 0.22);
}
</style>
