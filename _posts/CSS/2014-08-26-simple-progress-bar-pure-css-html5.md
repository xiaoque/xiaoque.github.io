---
layout: default
type: post
category: CSS
title: Simple progress bar using pure CSS and HTML5
description: 
tag: [HTML5, CSS]
---

{% highlight CSS %}

.progressBarMixin(@backgroundColor,@progressBarColor, @borderRadius){
  progress {
    background-color: @backgroundColor;
    border: none;
    border-radius: @borderRadius;
    -webkit-appearance: none;
  }
  progress::-webkit-progress-bar {
    background-color: @backgroundColor;
    padding: 3px;
    border-radius: @borderRadius;
  }
  progress::-webkit-progress-value {
    background-color: @progressBarColor;
    border-radius: @borderRadius;
  }
  progress::-moz-progress-bar {
    background-color: @backgroundColor;
    padding: 3px;
    border-radius: @borderRadius;
  }
}


.progressBarMixin(@colorGreyLight,@colorBlueElectric, 50px);
      progress {
        height: 25px;
        width: 220px;
      }
{% endhighlight %}