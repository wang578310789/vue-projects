<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>数据统计</el-breadcrumb-item>
      <el-breadcrumb-item>数据报表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图区域 -->
    <el-card>
      <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
      <div id="main" style="width: 750px; height:400px;"></div>
    </el-card>
  </div>
</template>

<script>
// 导入echarts    得先安装
import echarts from 'echarts'
import _ from 'lodash'
export default {
  data() {
    return {
      // 需要合并的数据
      options: {
        title: {
          text: '用户来源'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
                backgroundColor: '#E9EEF3'
            }
          }
        },
        xAxis: [
          {
            boundaryGap: false
          }
        ],
        yAxis: [
          {
            type: 'value'
          }
        ]
      }
    }
  },
  created() {},
  async mounted() {
    // 基于准备好的dom，初始化echarts实例
    var myChart = echarts.init(document.getElementById('main'));

    // 请求接口里的数据
    const { data: res } = await this.$http.get('reports/type/1')
    console.log(res.data)
    if (res.meta.status == 200) {
      // 指定图表的配置项和数据
      const result = _.merge(res.data, this.options)
      // 展示数据
      myChart.setOption(result);
    } else {
      this.$message.error('获取折线图数据失败！');
    }
  },
  methods: {}
}
</script>

<style lang="scss" scoped>
</style>