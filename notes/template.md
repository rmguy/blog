---
<script>
function carnLun_spOneOnClick() {
  document.getElementById("carnLun_spoiler1").style.display = "block";
  document.getElementById("carnLun_spoiler2").style.display = "none";
  document.getElementById("carnLun_spoiler3").style.display = "none";
}

function carnLun_spTwoOnClick() {
  document.getElementById("carnLun_spoiler2").style.display = "block";
  document.getElementById("carnLun_spoiler1").style.display = "none";
  document.getElementById("carnLun_spoiler3").style.display = "none";
}

function carnLun_spThreeOnClick() {
  document.getElementById("carnLun_spoiler3").style.display = "block";
  document.getElementById("carnLun_spoiler2").style.display = "none";
  document.getElementById("carnLun_spoiler1").style.display = "none";
}

</script>

<button onclick="carnLun_spThreeOnClick()" id="carnLun_spoilerButtonThree" style="display:block;opacity:1;background-color:Transparent; color:grey; border:none;">View drawing: <strong><em>Caption I</em></strong></button>

<div id="carnLun_spoiler3" style="display:none">
<img style="padding-right:100%;padding-bottom:20px" align="left" width=100% height=100% src=https://user-images.githubusercontent.com/3627706/83976551-37480f00-a918-11ea-8c58-4676d22febc2.jpg alt="Drawings. Mail me if it doesn't show up: 87profligate@gmail.com" />

<em style="font-size:14px">Say something</em>

</div>

<button onclick="carnLun_spTwoOnClick()" id="carnLun_spoilerButtonTwo" style="display:block;opacity:1;background-color:Transparent; color:grey; border:none;">View drawing: <strong><em>Caption II</em></strong></button>

<div id="carnLun_spoiler2" style="display:none">
<img style="padding-right:100%;padding-bottom:20px" align="left" width=100% height=100% src=https://user-images.githubusercontent.com/3627706/83976551-37480f00-a918-11ea-8c58-4676d22febc2.jpg alt="Drawings. Mail me if it doesn't show up: 87profligate@gmail.com" />

<em style="font-size:14px">Say something else</em>

</div>

---
<button onclick="carnLun_spOneOnClick()" id="carnLun_spoilerButtonOne" style="display:block;opacity:1;background-color:Transparent; color:grey; border:none;">Appendix (<strong><em>Caption III</strong></em>)</button>

<div id="carnLun_spoiler1" style="display:none">

Markdown -> html

</div>

---

Feel free to reproduce this story in whole or part without attributing the source.

