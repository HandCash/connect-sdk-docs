---
title: "Assets"
date: 2018-12-28T11:02:05+06:00
icon: "ti-image"
description: "Buttons, images, and more."
type : "docs"
weight: 6
---

#### Overview

While integrating Connect into your app, you may want to take advantage of some assets such as colors, buttons, or images.

<hr>
<br>

#### Login

When prompting your users to login with Connect, you can use the following button:

<br/>

{{< connect-button >}}

<br/>
<br/>


To add this button into your app, use the following html, js, and css:


 {{< tabs >}}
   {{% tab "HTML" %}}
```html
<head>
	<meta charset="utf-8">
	<link rel="stylesheet" href="css/styles.css">
</head>
<body>
	<div id="connectButton" url="<redirection-url>">Connect with HandCash</div>
	<script type="text/javascript" src="js/connect_button.js"></script>
</body>
```
   {{% /tab %}}
      {{% tab "JavaScript" %}}
```javascript
const connectButton = document.getElementById('connectButton');
const url = connectButton.getAttribute('url');

function connectWithHandCash() {
    location.href = url;
}

console.log(connectButton);

connectButton.addEventListener('click', connectWithHandCash, false);
```
   {{% /tab %}}
         {{% tab "CSS" %}}
```css
@charset "UTF-8";
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@500&display=swap');
#connectButton{
	-webkit-user-select: none; /* Safari */        
	-moz-user-select: none; /* Firefox */
	-ms-user-select: none; /* IE10+/Edge */
	user-select: none; /* Standard */
	font-family: 'Poppins', sans-serif;
	box-shadow: 0px 1px 3px hsla(0, 0%, 0%, .15);
	background-image: linear-gradient(#38CB7C, #1CB462);
	border-radius: 8px;
	display:inline-block;
	text-align: center;
	cursor:pointer;
	color:#ffffff;
	font-size:16px;
	font-weight:500;
	padding:16px 24px;
	text-decoration:none;
	transition: 0.3s;
	width: 100%;
	max-width: 320px;
	vertical-align: middle;
	letter-spacing: 0.5px;
}
#connectButton:before {
	background: url(https://handcash.io/resources/handcash_white_icon.svg) no-repeat scroll center center / 100% auto rgba(0, 0, 0, 0);
    content: "";
    display: inline-block;
    color: #fff;
    height: 20px;
    margin-right: 12px;
    margin-bottom: 1px;
    position: relative;
    vertical-align: middle;
    width: 20px;
}
#connectButton:hover {
	background-image: linear-gradient(#31C475, #16B15D);
	top:1px;
	box-shadow: 0px 3px 6px hsla(0, 0%, 0%, .15);
}
#connectButton:active {
	background-image: linear-gradient(#38CB7C, #1CB462);
	position:relative;
	top:1px;
	box-shadow: 0px 0px 0px hsla(0, 0%, 0%, .15);
}
```
   {{% /tab %}}
{{< /tabs >}}


You may also download the complete zip here: [Download](./handcash_connect_button.zip)

<hr>
<br>

#### Colors


Below is a collection of our brand colors, please feel free to use these as you see fit:

</br>


{{< card-grid w="200px" >}}
	{{< color color="#38CB7C" label="Primary Color" >}}
{{< /card-grid >}}

<hr>
<br>

#### Images

Below is a collection of image assets, you may use these anywhere you'd like:


 {{< tabs >}}
   {{< tab "Logos with Text" >}}

{{< card-grid  w="200px" >}}

	{{< grid-image image="connect_logo_text.png" >}}

	{{< grid-image image="handcash_logo_color.png" >}}

	{{< grid-image image="connect_logo_white_2x.png" >}}

	{{< grid-image image="handcash-white-wide.png" >}}

{{< /card-grid >}}


 {{< /tab >}}
 {{< tab "Square Logos" >}}
 {{< card-grid  w=100px >}}

	{{< grid-image image="handcash-icon-1024.png" >}}
	
	{{< grid-image image="handcashlogowhite.png" >}}
	
	{{< grid-image image="handcash_green_icon_2x.png" >}}
	
	{{< grid-image image="ic_stat_handcash_notification.png" >}}

{{< /card-grid >}}

