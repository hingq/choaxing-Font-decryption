var $ = unsafeWindow.jQuery
var  md5 = md5 || window.md5
var Typr = Typr || window.Typr
// 判断是否存在加密字体
var $tip = $('style:contains(font-cxsecret)')
if (!$tip.length) return;
// 解析font-cxsecret字体
var font = $tip.text().match(/base64,([\w\W]+?)'/)[1]
font = Typr.parse(base64ToUint8Array(font))[0];
// 匹配解密字体
var table = JSON.parse(GM_getResourceText('Table'))
var match = {};
const arr = []
$('.font-cxsecret').html(function (index, html) {
    //匹配中文字符
    var key = new RegExp(/[\u4e00-\u9fa5]/, 'g')
    if (hasLengthProperty(html.match(key))) {
        for (var j = 0; j < key.length; j++) {
            if (arr.indexOf(key[j].charCodeAt()) === -1) arr.push(key[j].charCodeAt())
        }
    }
})
for (var i = 0; i < arr.length; i++) { 
    var index = Typr.U.codeToGlyph(font, arr[i])
    $tip = Typr.U.glyphToPath(font, index);
    if (!$tip) continue;
    $tip = md5(JSON.stringify($tip)).slice(24)
    if (index) match[arr[i]] = table[$tip];
}
$('.font-cxsecret').html(function (index, html) {
    $.each(match, function (key, value) {
        key = String.fromCharCode(key);
        key = new RegExp(key, 'g');
        value = String.fromCharCode(value)
        html = html.replace(key, value)
    });
    return html;
}).removeClass('font-cxsecret'); 
function base64ToUint8Array(base64) {
    var data = window.atob(base64);
    var buffer = new Uint8Array(data.length);
    for (var i = 0; i < data.length; ++i) {
        buffer[i] = data.charCodeAt(i);
    }
    return buffer;
}
function hasLengthProperty(variable) {
    return variable != null && typeof variable.length !== 'undefined';
}
