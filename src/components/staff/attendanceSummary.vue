<template>
    <div>
        <visitor-pie :pieData="pieData"></visitor-pie>
    </div>
</template>

<script>
    import visitorPie from '../visitorPie'
    export default {
    	data(){
    		return {
    			pieData: {},
    		}
    	},
    	components: {
            visitorPie,
    	},
    	mounted(){
    		this.initData();
    	},
    	methods: {
    		async initData(){
    			try{
            this.pieData = {
              leaveCount:20,//请假天数
              overTimeCount:365,//加班天数
              attendanceCount:300,//出勤天数
              awayOfficialCount:100//出差天数
            }
    			
    			}catch(err){
    				console.log('获取数据失败',err);
    			}
    		},
    	}
    }
</script>

<style>
	
</style>
