define(['jquery','underscore','mage/utils/objects','mage/utils/strings'],function($,_,utils,stringUtils){'use strict';var tmplSettings=_.templateSettings,interpolate=/\$\{([\s\S]+?)\}/g,opener='${',template,hasStringTmpls;hasStringTmpls=(function(){var testString='var foo = "bar"; return `${ foo }` === foo';try{return Function(testString)();}catch(e){return false;}})();function isTmplIgnored(tmpl,target){var parsedTmpl;try{parsedTmpl=JSON.parse(tmpl);if(typeof parsedTmpl==='object'){return tmpl.includes('__disableTmpl');}}catch(e){}
if(typeof target!=='undefined'){if(typeof target==='object'&&target.hasOwnProperty('__disableTmpl')){return true;}}
return false;}
if(hasStringTmpls){template=function(tmpl,$,target){if(!isTmplIgnored(tmpl,target)){return eval('`'+tmpl+'`');}
return tmpl;};}else{template=function(tmpl,data){var cached=tmplSettings.interpolate;tmplSettings.interpolate=interpolate;tmpl=_.template(tmpl,{variable:'$'})(data);tmplSettings.interpolate=cached;return tmpl;};}
function isTemplate(value){return typeof value==='string'&&value.indexOf(opener)!==-1&&value.indexOf('${{')===-1;}
function render(tmpl,data,castString,target){var last=tmpl;while(~tmpl.indexOf(opener)){tmpl=template(tmpl,data,target);if(tmpl===last){break;}
last=tmpl;}
return castString?stringUtils.castString(tmpl):tmpl;}
return{template:function(tmpl,data,castString,dontClone){if(typeof tmpl==='string'){return render(tmpl,data,castString);}
if(!dontClone){tmpl=utils.copy(tmpl);}
tmpl.$data=data||{};_.each(tmpl,function iterate(value,key,list){if(key==='$data'){return;}
if(isTemplate(key)){delete list[key];key=render(key,tmpl);list[key]=value;}
if(isTemplate(value)){list[key]=render(value,tmpl,castString,list);}else if($.isPlainObject(value)||Array.isArray(value)){_.each(value,iterate);}});delete tmpl.$data;return tmpl;}};});