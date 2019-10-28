<template>
    <div>
        <h2>线性进度条---外显</h2>
        <lhp-progress :percentage="0" :showText='false'></lhp-progress>
        <lhp-progress :percentage="50" color='orange'></lhp-progress>
        <lhp-progress :percentage="60" status='success'></lhp-progress>
        <lhp-progress :percentage="80" status = 'exception'></lhp-progress>
        <h2>线性进度条---外显</h2>
        <lhp-progress :percentage="0" :strokeWidth = '18' :textInside='true' :showText = 'false'></lhp-progress>
        <lhp-progress :percentage="40" :strokeWidth = '18' :textInside='true'></lhp-progress>
        <lhp-progress :percentage="60" :strokeWidth = '18' :textInside='true' status='success'></lhp-progress>
        <lhp-progress :percentage="80" :strokeWidth = '18' :textInside='true' status='exception'></lhp-progress>
        <h2>环形进度条</h2>
        <lhp-progress :percentage="0" type='circle'></lhp-progress>
        <lhp-progress :percentage="40" type='circle'></lhp-progress>
        <lhp-progress :percentage="60" type='circle' status = 'success'></lhp-progress>
        <lhp-progress :percentage="80" type='circle' status = 'exception'></lhp-progress>

    </div>
</template>
<script>
import LhpProgress from "./progress"
export default {
    components:{
        LhpProgress
    }

}
</script>