---
layout: post
title: "Delay một action nào đó của js"
date: 2017-05-26 10:00 PM
categories: [Javascript]
author: yeungon
tags : [javascript, js]
comments: true
---
## Code sau dùng để delaying một action nào đó của js

<?php
//code 1
<script>
function debounce(func, wait) {
var timeout;
return function () {
var context = this, args = arguments;
var later = function () {
func.apply(context, args);
}
clearTimeout(timeout);
timeout = setTimeout(later, wait);
}
}
</script>
//function này giống cái trên nhé, param function này bạn truyền tên hàm này vào nhé
//code 2 //http://stackoverflow.com/questions/7849221/ajax-delay-for-search-on-typing-in-form-field
<script>
var delayTimer;
function doSearch(text) {
    clearTimeout(delayTimer);
    delayTimer = setTimeout(function() {
        // Do the ajax stuff
    }, 1000); // Will do the ajax stuff after 1000 ms, or 1 s
}
</script>
// code 3: 
// below is the html form
<form action="" method="post" accept-charset="utf-8">
    <p><input type="text" name="q" id="q" value="" onkeyup="doDelayedSearch(this.value)" /></p>
</form>
// script to handle the above html form
<script>
var timeout = null;
function doDelayedSearch(val) {
  if (timeout) {  
    clearTimeout(timeout);
  }
  timeout = setTimeout(function() {
     doSearch(val); //this is your existing function
  }, 2000);
}
</script>
// code 4: http://remysharp.com/2010/07/21/throttling-function-calls/
    
 //code 5:
    
var i = 1;                     //  set your counter to 1
function myLoop () {           //  create a loop function
   setTimeout(function () {    //  call a 3s setTimeout when the loop is called
      alert('hello');          //  your code here
      i++;                     //  increment the counter
      if (i < 10) {            //  if the counter < 10, call the loop function
         myLoop();             //  ..  again which will trigger another 
      }                        //  ..  setTimeout()
   }, 3000)
}
myLoop();                      //  start the loop
//https://stackoverflow.com/questions/3583724/how-do-i-add-a-delay-in-a-javascript-loop
//code  6
function postData(){
    var id = $('#id').val();
    $.post('inc/repairs/events-backend.php',{id:id},   
        function(data){
        $("#display_customer").html(data);
    });
    return false;
}
$(function() {
    var timer;
    $("#id").bind('keyup input',function() {
        timer && clearTimeout(timer);
        timer = setTimeout(postData, 300);
    });
});
// https://stackoverflow.com/questions/14471889/jquery-keyup-delay
// kiểm tra keyword có được đưa vào không, abort
//https://stackoverflow.com/questions/7528050/live-search-optimisation-in-javascript
//https://stackoverflow.com/questions/7373023/throttle-event-calls-in-jquery
//http://code.google.com/p/jquery-debounce/
//Take a look at jQuery Debounce.
$('#search').keyup($.debounce(function() {
    // Will only execute 300ms after the last keypress.
}, 300));
// đây: sau khi typing: https://stackoverflow.com/questions/7373023/throttle-event-calls-in-jquery
//http://jsfiddle.net/josh3736/Xnnmf/
?>
