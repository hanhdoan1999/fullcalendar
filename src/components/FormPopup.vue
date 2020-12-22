<template lang="">
    <div>
        <el-dialog title="Add" :visible.sync="dialogFormVisible"> 
            <el-form ref="form" :model="form" :label-position="labelPosition" label-width="120px">
                <el-form-item label="Tên công việc">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="Ưu tiên" >
                    <el-select v-model="form.priority" placeholder="Chọn mức đọ ưu tiên" :label-position="labelPosition"> 
                    <el-option label="Cao" value="shanghai"></el-option>
                    <el-option label="Thấp" value="beijing"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="Bắt đầu">
                     <el-date-picker
                        v-model="form.start"
                        type="datetime"
                        range-separator="To"
                        start-placeholder="Start date"
                        end-placeholder="End date"
                        align="right">
                        </el-date-picker>
                </el-form-item>

                <el-form-item label="Kết thúc">
                     <el-date-picker
                        v-model="form.end"
                        type="datetime"
                        range-separator="To"
                        start-placeholder="Start date"
                        end-placeholder="End date"
                        align="right">
                        </el-date-picker>
                </el-form-item>
                <el-form-item label="Mô tả">
                    <el-input type="textarea" v-model="form.desc"></el-input>
                </el-form-item>
                <!-- <el-form-item>
                    <el-button type="primary" @click="onSubmit">Create</el-button>
                    <el-button>Cancel</el-button>
                </el-form-item> -->
            </el-form>
            <span slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">Cancel</el-button>
            <el-button type="primary" @click="save;dialogFormVisible = false">Confirm</el-button>  
  
        </span>
        </el-dialog>



    </div>
</template>
<script>
export default {
  props: {
    dialogFormVisible: {
      type: Boolean,
    },

    form: {
      type: Object,
      default: () => ({}),
    },
  },

  data() {
    return {
      labelPosition: "left",
      // dialogFormVisible: false,
      // form: {
      //   name: '',
      //   priority: '',
      //   start: '',
      //   time: '',
      //   desc: '',
      // },
      formLabelWidth: "120px",
    };
  },
  methods: {
    save() {
      console.log("adfas");
      this.$emit("form-popup-save", this.form);
      this.dialogFormVisible  = false
    },
  },
};
</script>
<style lang="">
</style>