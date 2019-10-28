<template>
    <div class="progress" :class="[`progressType${type}`]">
        <div class="progress_bar" 
            v-if="type==='line'">
            <div class="progress--outer" :style="{height:strokeWidth+'px'}">
                <div class="progress--inner" :style="{width:percentage+'%',backgroundColor:stroke}">  
                    <div class="progress--inner__text"
                        v-if='textInside && showText'>
                        {{percentage}}%
                    </div>
                </div>
            </div>
        </div>
        <div class="progress-circle"
            :style="{width: width+'px',height: width+'px'}"
            v-else>
            <svg viewBox="0 0 100 100">
                <path
                    :d="circleStrokePath"
                    fill="none"
                    :stroke = "stroke"
                    :stroke-width ="strokeWidth"
                    stroke-linecap='round'
                    :style = "circleStrokeStyle"
                >
                </path>

            </svg>

        </div>
        <div class="progress_text" 
             :style="{fontSize:progressTextFontSize+'px'}"
             v-if="!textInside && showText">
           <template v-if="!status"> {{percentage}}% </template>
           <i v-else class='iconfont' :class="iconClass" :style='{color:stroke}'></i>

        </div>
    </div>
</template>
<script>
export default {
    props:{
        strokeWidth:{
            type:Number,
            default:6
        },
        percentage:{
            type:Number,
            default:0,
            required:true,
            validator(val){
                return val>=0 && val<=100
            }
        },
        status:{
            type:String
        },
        type:{
            type:String,
            default:'line',
            validator:val=>['line','circle'].includes(val)
        },
        textInside:{
            type:Boolean,
            default:false,
        },
        showText:{
            type:Boolean,
            default:true
        },
        width:{
            type:Number,
            default:126
        },
        color:{
            type:String
        }

    },
    computed:{
        progressTextFontSize(){
            return 12+this.strokeWidth*0.4;
        },
        stroke(){
            let color;
            if(this.color){
                return this.color;
            }
            switch(this.status){
                case 'success': color = '#13ce66';break;
                case 'exception':color = '#f56c6c';break;
                default:color = '#20a0ff';
            }
            return color;
        },
        iconClass(){
            if(this.type==='line'){
                return this.status === 'success'? 
                'icon-circle-check':'icon-circle-close';
                
            }else{
                return this.status === 'success'? 
                'icon-check':'icon-close';
            }
            
        },

        circleStrokePath(){
            const radius = 50 - this.circleStrokeWidth/2;
            return `
                M 50 50
                m 0 -${radius}
                a ${radius} ${radius} 0 1 1 0 ${radius*2}
                a ${radius} ${radius} 0 1 1 0 -${radius*2}

            `
        },
        circleStrokeWidth(){
            return 100*this.strokeWidth/this.width;
        },
        perimeter(){
            const radius = 50 - this.circleStrokeWidth/2;
            return 2*Math.PI*radius;
        },
        circleStrokeStyle(){
            const perimeter = this.perimeter;
            return {
                strokeDasharray: `${perimeter}px ${perimeter}px`,
                strokeDashoffset:  (1-this.percentage/100)*perimeter+'px'
                

                
            }
        }
        
    }


}
</script>
<style >
    /* 进度条 */
    @font-face {
        font-family: 'iconfont';  /* project id 1477732 */
        src: url('./iconfont/iconfont.eot');
        src: url('./iconfont/iconfont.eot?#iefix') format('embedded-opentype'),
        url('./iconfont/iconfont.woff2') format('woff2'),
        url('./iconfont/iconfont.woff') format('woff'),
        url('./iconfont/iconfont.ttf') format('truetype'),
        url('./iconfont/iconfont.svg#iconfont') format('svg');
    }
    .iconfont {
        font-family: "iconfont" !important;
        font-size: 16px;
        font-style: normal;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }
    .icon-circle-check::before{
        content:'\e770'
    }
    .icon-circle-close::before{
        content:'\e630'
    }
    .icon-check::before{
        content:'\e633'
    }
    .icon-close::before{
        content:'\e625'
    }

    .progress_bar{
        display:inline-block;
        width:100%;
        box-sizing: border-box;
        padding-right:50px;
        margin-right:-50px;

    }
    .progress--outer{
        border-radius: 100px;
        background-color:#ebeef5;
    }
    .progress--inner{
        height: 100%;
        border-radius: 100px;
        background-color:red;
        transition:width 0.6s ease;
        line-height: 1;
        text-align: right;

    }
    .progress--inner__text{
        display: inline-block;
        color:#fff;
        margin-right:5px;
        vertical-align: middle;
        font-size:12px;

    }

    /* 文字 */
    .progress_text{
        display:inline-block;
        margin-left:10px;

    }
    .progressTypecircle{
        position:relative;
        display: inline-block;
    }
    .progressTypecircle .progress_text{
        position:absolute;
        display: inline-block;
        top:50%;
        left:50%;
        text-align: center;
        transform:translateX(-50%) translateY(-50%);
        margin-left:0;
    }
    

</style>