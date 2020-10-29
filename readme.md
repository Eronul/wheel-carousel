Carousel点击按钮翻页效果页面 代码：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Carousel</title>
    
    <style>
        .container {
           max-width: 800px;
           margin: 30px auto;
           border-radius: 4px;
           box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.3);
           padding: 16px;
        }   

        .carousel {
           position: relative;   /*相对容器绝对定位*/
           height: 200px;
        }

        .carousel .panels > a {
           position: absolute; 
           display: flex;
           align-items: center;   /*因为有文字*/
           justify-content: center;
           width: 100%;
           height: 100%;    /*高度由父亲 .carousel 的 height 控制*/
           text-decoration: none;
           opacity: 0;
           z-index: 0;
           transition: all .3s;
        }

        .carousel .panels > a.active {
            opacity: 1;
            z-index: 1;
    }

        .carousel .panels > a:nth-child(even) {
            background-color: lightskyblue
        }

        .carousel .panels > a:nth-child(odd) {
            background-color: lightpink
        }

        .carousel .arrow {
            position: absolute;
            top: 50%;
            z-index: 100;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 32px;
            height: 32px;
            border: none;     /*把点击的按钮去掉黑色边框，变成透明色*/
            border-radius: 50%;   /*变成圆*/
            background: rgba(31,45,61,.11);
            opacity: 0;
            transition: all .3s;
            outline: none;
            cursor: pointer;
        }

        .carousel .arrow-pre {
            left: 10px;
            transform: translateX(-10px) translateY(-50%);
        }

        .carousel:hover .arrow-pre {
            transform: translateX(0) translateY(-50%);
            opacity: 1;
        }

        .carousel .arrow-next {
            right: 10px;
            transform: translateX(10px) translateY(-50%);            
        }

        .carousel:hover .arrow-next {
            transform: translateX(0) translateY(-50%);
            opacity: 1;
        }

        .carousel .arrow::before {
            content: '';
            display: block;
            width: 6px;
            height: 6px;   
            border-left: 1px solid #fff;
            border-top: 1px solid #fff;
            transform: rotate(-45deg);
        }

        .carousel .arrow.arrow-next::before {
            transform: rotate(135deg);
        }

        .carousel .indicators {
            position: absolute;
            z-index: 100;
            left: 50%;                    /*绝对定位的水平居中*/ 
            bottom: 10px; 
            transform: translateX(-50%);  /*绝对定位的居中*/
            list-style: none;    /*消除4个li的点样式*/
            margin: 0;
            padding: 0;
        }

        .carousel .indicators > li {    /* 把之前加的 li 的样式变成伪元素，然后把 li 变成父亲加pandding  扩大用户鼠标点击范围 */
            display: inline-block;
            padding: 5px 0;   /*上下5px  左右为 0 */
            cursor: pointer;
        }

        .carousel .indicators > li::before {     /*变成伪元素*/
            content: '';
            display: block;                  
            width: 30px;
            height: 2px;
            border-radius: 2px;
            background: #c0c4cc;
            transition: all .3s;
        }

        .carousel .indicators > li.active::before {
            background-color: #fff;
        }

    </style>
</head>
<body>
    <div class="container">
    <h2>Carousel</h2>
    <div class="carousel">
      <div class="panels">
        <a class="active" href="#0">0</a>
        <a href="#1">1</a>
        <a href="#2">2</a>
        <a href="#3">3</a>
      </div>
      <div class="arrows">
        <button class="arrow arrow-pre"></button>
        <button class="arrow arrow-next"></button>
      </div>
      <ul class="indicators">
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </div>

    </div>

    <script>
        
        const $ = s => document.querySelector(s)
        const $$ = s => document.querySelectorAll(s) /*不加引号*/

        const $pre = $('.carousel .arrow-pre')
        const $next = $('.carousel .arrow-next')
        const $$indicators = $$('.carousel .indicators > li')
        const $$panels = $$('.carousel .panels > a')

        const getIndex = () => [...$$indicators].indexOf($('.carousel .indicators .active'))
        const getPreIndex = () => (getIndex() - 1 + $$indicators.length) % $$indicators.length
        const getNextIndex = () => (getIndex() + 1) % $$indicators.length

        const setPage = index => {
            $$panels.forEach($panel => $panel.classList.remove('active'))
            $$panels[index].classList.add('active')
        }
        const setIndicator = index => {
            $$indicators.forEach($indicator => $indicator.classList.remove('active'))
            $$indicators[index].classList.add('active')
        }

        $pre.onclick = function() {
            let index = getPreIndex()
            setPage(index)
            setIndicator(index)
        }

        $next.onclick = function() {
            let index = getNextIndex()
            setPage(index)
            setIndicator(index)
        }

        $$indicators.forEach($indicator => $indicator.onclick = function(e) {
            let index = [...$$indicators].indexOf(e.target)
            setIndicator(index)
            setPage(index)
        })    /*类数组不能直接绑定事件，需要遍历对象*/



    </script>

</body>
</html>
