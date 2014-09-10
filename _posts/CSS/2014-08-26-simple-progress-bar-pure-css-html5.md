---
layout: default
type: post
category: CSS
title: Simple progress bar using pure CSS and HTML5
description: 
tag: [HTML5, CSS]
---


HTML5 introduced a new element __progress__ , which make it much easier to show progess bar. In this article, I just wanna share my experience to manipulate this element. For different browser's support check this [Can I use ?](http://caniuse.com/#feat=progressmeter)    



___
###Basic Usage
The progress bar can be added using &lt;progress> ; the progress value is determined by the value, min and max attributes.

{% highlight html %}
<progress value="10" max="30"/>
{% endhighlight %} 
 
<progress value="10" max="30"/>

The presentation is denpending on your system.     


___
###Styling it
Well, I want a progress bar with some border-radius, it not complicated. just need to pay attention to different browsers' process with progress. 

In __Chrome__ and __Safari__, the progress element is translate in this way

{% highlight css %}
progress::-webkit-progress-bar {  
    /* style rules */  
}  
progress::-webkit-progress-value {  
    /* style rules */  
}  
{% endhighlight %}  

And in Firfox it also has its special pseudo-class for progress  

{% highlight css %}
progress::-moz-progress-bar {  
    /* style rules */  
}  
{% endhighlight %}  


With ReactJS, I use .less, a CSS pre-processor, I want to reuse the style of this progress bar just with different background color, and progress color, so I defined a mixin, as follow.


{% highlight css %}
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
{% endhighlight %}  


{% highlight css %}
.progressBarMixin(@colorGreyLight,@colorBlueElectric, 50px);
      progress {
        height: 25px;
        width: 220px;
      }
{% endhighlight %} 

  
  ___
###Animation
Because I use [ReactJS](http://facebook.github.io/react/) to render my application, it has a great function 

{% highlight javascript %}
boolean shouldComponentUpdate(object nextProps, object nextState)
{% endhighlight %} 

This function returns true by default, that means it will update your layout automatically when it detects some changes have been made.  
So I got a function to caculate the progress like this.

{% highlight javascript %}
getProgressBarValue: function () {
        var finished = 0;
        var finishedSteps = this.props.storeCursor.follow("finishedSteps").get();
        _.each(steps.GROUP_NAMES, function (name) {
            if(steps.isStepGroupFinished(name, finishedSteps))
            finished ++;
        });
        return (finished / OnboardingStepName.length);
    }
{% endhighlight %} 
{% highlight html %}
<progress value={this.getProgressBarValue()} max="1"></progress>
{% endhighlight %} 

Finally, I got my progress bar.

![image]({{site.img-url}}/progress-bar.png)

Hope this can help you!