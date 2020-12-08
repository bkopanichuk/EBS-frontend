<template>
  <el-row style="padding: 20px; box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1); margin: 20px">
    <b style="font-size: 32px">Data clip</b>
    <el-col :span="24" style="display: flex; justify-content: center; padding-top: 20px">
      <div class="block">
        <el-select v-model="measurement" multiple placeholder="Measurement" >
          <el-option
              v-for="indication in deviceInfo.device_model.indications"
              :key="indication.measurement"
              :label="indication.measurement"
              :value="indication.measurement">
          </el-option>
        </el-select>
        <el-date-picker
            v-model="date"
            type="datetimerange"
            :picker-options="pickerOptions"
            @change="set_date"
            range-separator="To"
            start-placeholder="Start date"
            end-placeholder="End date"
            style="margin-left: 20px"
            align="right">
        </el-date-picker>
        <AreaChart :key="chart_draw_flag" width="1200px" v-bind:label_set="label_set" v-bind:values="values"/>
      </div>
    </el-col>
  </el-row>
</template>

<script>
import AreaChart from "@/components/BoardComponents/AreaChart";

export default {
  name: "DataClip",

  methods: {
    set_date: async function () {
      var tmpString = this.date[0].toJSON() + ''
      this.date[0] = tmpString.split('.')[0]
      tmpString = this.date[1].toJSON() + ''
      this.date[1] = tmpString.split('.')[0]

      var response = await fetch('https://meter-solutions.herokuapp.com/api/device/' + this.deviceId + '/get_data_by_date/?start-date=' + this.date[0] + '&end-date=' + this.date[1] + '&measurement=' + this.measurement, {
        headers: {
          'Authorization': 'Bearer ' + sessionStorage.getItem('token'),
        },
      })

      this.dataclip = await response.text()
      this.dataclip = JSON.parse(this.dataclip)

      this.label_set = []
      this.values = []
      console.log(this.dataclip)
      for (var i = 0; i < this.dataclip.length; i++) {
        this.label_set.push(this.dataclip[i].timestamp)
        this.values.push(this.dataclip[i].value)
      }
      console.log(this.label_set)
      console.log(this.values)

      this.chart_draw_flag = !this.chart_draw_flag
    }
  },

  components: {
    AreaChart,
  },

  props: {
    deviceId: {
      type: String,
      required: true
    },
    deviceInfo: Object,
  },

  data() {
    return {
      label_set: [],
      values: [],
      chart_draw_flag: true,
      pickerOptions: {
        shortcuts: [{
          text: 'Last week',
          onClick(picker) {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
            picker.$emit('pick', [start, end]);
          }
        }, {
          text: 'Last month',
          onClick(picker) {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
            picker.$emit('pick', [start, end]);
          }
        }, {
          text: 'Last 3 months',
          onClick(picker) {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
            picker.$emit('pick', [start, end]);
          }
        }]
      },
      measurement: '',
      date: '',
      dataclip: [],
    }
  },

  mounted() {
    //fetch('http://0.0.0.0:9999/api/device/' + this.deviceId + '/get_data_by_date/?start-date='+  + "&end-date=" +)
  }
}
</script>

<style scoped>

</style>