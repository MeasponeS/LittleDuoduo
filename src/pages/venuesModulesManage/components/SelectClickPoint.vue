<template>
    <el-dialog
        title="选择点击区域"
        :visible.sync="visible"
        width="736px"
        @open="open"
        :modal="false"
        :close-on-click-modal="false"
        :show-close="false">
        <div class="imgs">
            <img :src="image" alt="" class="pointImg" @click="savePosition">
        </div>
        <div class="points" v-if="left || top" :style="{'left': (left + 32 ) + 'px', 'top': (top + 76) + 'px'  }">
            <i class="iconfont">&#xe7ad;</i>
        </div>
        <span slot="footer" class="dialog-footer">
            <el-button @click="closeDialog">取 消</el-button>
            <el-button type="primary" @click="submit">确 定</el-button>
        </span>
    </el-dialog>
</template>

<script>
	export default {
		name: "SelectClickPoint",
        props: ['visible', 'image','position'],
        data (){
		    return {
		    	left: '',
                top: ''
            }
        },
        methods: {
			open(){
				if (this.position.length) {
					this.left = this.position[0];
					this.top = this.position[1]
				}
			},
			savePosition(e) {
				this.left = e.offsetX;
				this.top = e.offsetY;
			},
			closeDialog () {
                this.$emit('close')
            },
            submit () {
				if (!this.left || !this.top) {
					this.$message.error({message: '请在地图选择点击位置(边框除外)',center: true});
					return
                }
				this.$emit('save',[this.left, this.top]);
			},
        }
	}
</script>

<style scoped lang="scss">
    .pointImg{
        width: 656px;
        height: auto;
    }
    .el-dialog__body{
        position: relative;
        .points{
            position: absolute;
            i{
                color: red;
            }
        }
    }

</style>
