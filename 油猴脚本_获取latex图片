// ==UserScript==
// @name         latex获取真实下载链接
// @namespace    https://www.latexlive.com/home
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://www.latexlive.com/
// @icon         https://www.google.com/s2/favicons?sz=64&domain=latexlive.com
// @grant        none
// @require https://code.jquery.com/jquery-2.1.4.mim.js
// ==/UserScript==



(function () {
    'use strict';
    $(function () { console.log('引入jquery成功！') })
    $("#btn_png").ready(function () { //当btn_png加载成功再执行
        var btn_png = document.querySelector("#btn_png");
        var btn = document.createElement('button')
        btn.innerHTML = '复制';
        btn_png.before(btn);
        $('#wrap_output > mjx-container > svg').ready(function(){  
            btn.addEventListener('click', (e) => {
                var svg = document.querySelector('#wrap_output > mjx-container > svg');
                if (svg == (null|| ''||"")){
                    console.log('获取失败！')
                    return;
                }
                var downloadPNG = function (svgsource) {
                    svgsource.setAttribute("width", "1920px");
                    svgsource.setAttribute("height", "1080px");
                    var svgXml = svgsource.outerHTML;
                    var image = new Image();
                    image.src = "data:image/svg+xml;base64," + window.btoa(unescape(encodeURIComponent(svgXml)));
                    image.onload = function () {
                        var canvas = document.createElement("canvas");
                        canvas.width = 3840;
                        canvas.height = 2160;
                        var context = canvas.getContext("2d");
                        context.drawImage(image, 0, 0, 3840, 2160); //裁剪透明像素并加边儿
                        var imgData = canvas.getContext("2d").getImageData(0, 0, 3840, 2160).data;
                        var lOffset = canvas.width,
                            rOffset = 0,
                            tOffset = canvas.height,
                            bOffset = 0;
                        for (var i = 0; i < canvas.width; i++) {
                            for (var j = 0; j < canvas.height; j++) {
                                var pos = (i + canvas.width * j) * 4;
                                if (imgData[pos] > 0 || imgData[pos + 1] > 0 || imgData[pos + 2] || imgData[pos + 3] > 0) {
                                    bOffset = Math.max(j, bOffset); // 找到有色彩的最底部的纵坐标
                                    rOffset = Math.max(i, rOffset); // 找到有色彩的最右端
                                    tOffset = Math.min(j, tOffset); // 找到有色彩的最上端
                                    lOffset = Math.min(i, lOffset); // 找到有色彩的最左端
                                }
                            }
                        }
                        lOffset++;
                        rOffset++;
                        tOffset++;
                        bOffset++;
                        var canvas2 = document.createElement("canvas");
                        canvas2.width = rOffset - lOffset + 100;
                        canvas2.height = bOffset - tOffset + 100;
                        var context2 = canvas2.getContext("2d");
                        var w = canvas2.width;
                        var h = canvas2.height;
                        var ow = 50;
                        var oh = 50;
                        var nw = canvas2.width;
                        var nh = canvas2.height;
                        context2.drawImage(canvas, lOffset, tOffset, w, h, ow, oh, nw, nh);
                        var hiddenLink = document.createElement("a");
                        if (1) {
                            hiddenLink.href = canvas2.toDataURL("image/png");
                        } else {
                            hiddenLink.href = canvas.toDataURL("image/png");
                        }
                        console.log(hiddenLink.href);
                        hiddenLink.download = 'MommyTalk' + new Date().getTime() + ".png";
                        hiddenLink.click();
                    }}
                downloadPNG(svg);
            })
        })
    });

    // Your code here...
})();
