# timechunk
分时函数：作用：用于加载比较大的数据时分批加载数据
 //分时函数 入参：数据源,执行的函数，分次，多长时间加载下次
 
    var timeChunk=function(data,fn,times,wait){
        var obj,timer;
        var start=function(){
            var len=Math.min(times,data.length);
            for(var i=0;i<len;i++){
                var val=data.shift()
                fn(val);
            }
        }
        return function(){
            timer=setInterval(function(){
                if(data.length===0){
                    return clearInterval(timer);
                }
                start();
            },wait)
        }
    }


    var len=100;
    var arr=[];
    for(var i=0;i<len;i++){
        arr.push(i);
    }
    var render=timeChunk(arr,function(n){
        var oDiv=document.createElement('div');
        oDiv.innerHTML=n;
        document.body.appendChild(oDiv);
    },10,1000);
    render();
