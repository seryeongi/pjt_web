<template>
  <div>
    <layout/>
    <el-radio-group v-model="ui" class="mt20">
      <el-radio-button label="html">html</el-radio-button>
      <el-radio-button label="element">element-ui</el-radio-button>
    </el-radio-group>
    <div v-if="ui=='html'">
      <div class="divLeft">
        <input type="text" class="searchInput" v-model="form.name" placeholder="코드명" style="display:table-cell;width:90%"/>
        <input class="searchBtn" type="button" @click="doList" value="조회"/>
      </div>
      <div class="mt10" style="text-align:left">
        <span>대표코드</span>
        <input type="button" style="float: right; padding: 3px 0" value="등록"/>
      </div>
      <table style="width:100%" class="mt10">
        <colgroup>
          <col width="10%">
          <col width="60%">
          <col width="10%">
          <col width="20%">
        </colgroup>
        <thead>
          <td scope="col">코드</td>
          <td scope="col">코드명</td>
          <td scope="col">코드영문명</td>
          <td scope="col">코드순서</td>
          <td scope="col">사용여부</td>
          <td scope="col">연결코드1</td>
          <td scope="col">연결코드2</td>
          <td scope="col">연결코드3</td>

        </thead>
        <tbody>
          <tr v-for="item in comm_info" :key="item.comm_cd_id">
            <td>{{item.comm_cd_id}}</td>
            <td>{{item.comm_cd_nm}}</td>
            <td>{{item.comm_cd_eng}}</td>
            <td>{{item.cd_sort}}</td>
            <td>{{item.cd_use_yn}}</td>
            <td>{{item.conn_1_cd}}</td>
            <td>{{item.conn_2_cd}}</td>
            <td>{{item.conn_3_cd}}</td>

          </tr>
        </tbody>
      </table>
    </div>
    <div v-if="ui=='element'" class="mt10">
      <div class="divLeft">
        <el-input v-model="form.name" placeholder="제목" style="width:90%"></el-input>
        <el-button @click="doList" type="primary">조회</el-button>
      </div>
      <el-card class="box-card mt10">
        <div slot="header" class="clearfix" style="text-align:left">
          <span>공지사항</span>
          <el-button style="float: right; padding: 3px 0" type="text">등록</el-button>
        </div>
        <el-row type="flex">
          <el-table :data="noticeList" border highlight-current-row @row-click="rowSel" height="500">
            <el-table-column align="center" label="코드" width="80">
              <template slot-scope="scope">
                {{scope.row.comm_cd_id}}
              </template>
            </el-table-column>
            <el-table-column label="코드명" align="left" width="700">
              <template slot-scope="scope">
                {{scope.row.comm_cd_nm}}
              </template>
            </el-table-column>
            <el-table-column label="코드영문명" align="center" width="150">
              <template slot-scope="scope">
                {{scope.row.comm_cd_eng}}
              </template>
            </el-table-column>
            <el-table-column label="코드순서" align="center" width="148">
              <template slot-scope="scope">
                {{scope.row.cd_sort}}
              </template>
            </el-table-column>
            <el-table-column label="사용여부" align="center" width="148">
              <template slot-scope="scope">
                {{scope.row.cd_use_yn}}
              </template>
            </el-table-column>
            <el-table-column label="연결코드1" align="center" width="148">
              <template slot-scope="scope">
                {{scope.row.conn_1_cd}}
              </template>
            </el-table-column>
            <el-table-column label="연결코드2" align="center" width="148">
              <template slot-scope="scope">
                {{scope.row.conn_2_cd}}
              </template>
            </el-table-column>
            <el-table-column label="연결코드3" align="center" width="148">
              <template slot-scope="scope">
                {{scope.row.conn_3_cd}}
              </template>
            </el-table-column>
          </el-table>
          <el-form :data="noticeForm" style="width: 68%;margin-left:20px;">
            <el-form-item label="제목">
              <el-input v-model="noticeForm.subject"></el-input>
            </el-form-item>
            <el-form-item label="내용">
              <el-input type="textarea" :rows="17" v-model="noticeForm.ctnt"></el-input>
            </el-form-item>
          </el-form>
        </el-row>
      </el-card>
    </div>
  </div>
</template>

<script>
import layout from '@/views/layout/Layout'
import {reqPost} from '@/api/tran'

export default {
  name: 'Notice',
  components: { layout },
  data () {
    return {
      form: {
        subject: '',
        statCd: ''
      },
      noticeList: [],
      noticeForm: {},
      ui: 'html'
    }
  },
  methods: {
    doList () {
      reqPost('/notice/selectNotice', this.form).then(response => {
        this.noticeList = response.data.list
      })
    },
    rowSel (a, b, c) {
      this.noticeForm = a
    }
  }
}
</script>
