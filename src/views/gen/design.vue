<template>
  <div class="webapp">
    <basic-container>
      <avue-form-design :option="option"
                        @submit="handleSubmit">
      </avue-form-design>
    </basic-container>
  </div>
</template>


<script>
    import {getForm, postForm} from '@/api/gen/gen'
    import {validatenull} from "../../util/validate";

    export default {
        data() {
            return {
                box: true,
                option: {
                    column: []
                }
            }
        },
        created() {
            this.getFormInfo()
        },
        methods: {
            handleSubmit(json) {
                let params = this.$route.query;
                if (validatenull(params)) {
                    return false
                }
                let result = JSON.stringify(json)
                postForm(result, params.tableName, params.dsId).then(response => {
                    this.$message.success('生成并保存成功')
                })
            },
            getFormInfo() {
                let params = this.$route.query;
                if (validatenull(params)) {
                    return false
                }
                getForm(params.tableName, params.dsId).then(response => {
                    if (!validatenull(response.data.data)) {
                        this.option = JSON.parse(response.data.data)
                    }
                })
            }
        }
    }
</script>
<style lang="scss">
  .webapp {
    background-color: #fff;
    position: relative;
    width: 100%;
    height: 100%;

    .form-designer {
      height: 800px;
    }

    .form-designer .widget-config-container .el-tabs__header {
      position: relative;
    }
  }
</style>
