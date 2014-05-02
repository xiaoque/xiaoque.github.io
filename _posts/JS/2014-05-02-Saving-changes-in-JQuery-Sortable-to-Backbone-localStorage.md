---
layout: default
type: post
category: JS
title: Saving changes in jQuery Sortable to Backbone localStorage
tag: [jQuery,Backbone,Sortable,LocalStorage]
---

{% include JB/setup %}

> The aim is to using jQuery Sortable to show models stored in localStorage and saving the sequence after changing.

###jQuery Sortable
***
Jquery Sortable is a flexible, opinionated sorting plugin for jQuery, you can see an official demo here [jQuey sortable](https://jqueryui.com/sortable/), also there is a site that explain it very clearly, I found it's very useful [jQuery Sortable](http://johnny.github.io/jquery-sortable/).



###Getting started
***

First is to define Model, Collection, View.

#####Define Model
{% highlight js linenos=table %}  
var Content = Backbone.Model.extend({  
	defaults: function () {  
		return {  
			text: "",  
			order: collection.nextOrder()  
		};  
	},  
	
	initialize: function () {  
		if (!this.get("text")) {  
			this.set({"text": "empty", "title": "empty"});  
        }  
    });  
{% endhighlight %} 

#####Define Collection
{% highlight js linenos=table %}  
var ContentCollection = Backbone.Collection.extend({  
	model: Content,  
	this.localStorage: new Backbone.LocalStorage("contentList"),  
	nextOrder: function(){  
		if (!this.length) return 1;  
		return this.last().get('order') + 1;  
	},  

	comparator: function(model) {  
		return model.get('order');  
	},  
	
	reOrder: function(){  
		var length = this.length;  
		for(var i = 0; i<length ; i++){  
			this.models[i].save({"order": i});  
		}  
	} 
});  
{% endhighlight %}

_Note: the comparator is to sort models in collection_

#####Define View
{% highlight js linenos=table %}  
	var ContentView = Backbone.View.extend({  
        tagName: 'li',  
        template: _.template($('#searchTemplate').html()),  

        render: function() {  
            this.$el.html( this.template( this.model.toJSON() ) );  
            return this;  
        }  

    });

{% endhighlight %}

This is the view shows each single model

#####Define AppView
{% highlight js linenos=table %}  
var AppView = Backbone.View.extend({  
	el: $("#contents"),  
	
	initialize: function(){  
		_.bindAll(this, 'add', 'addAll');  
		this.collection.bind('add', this.add);  
		this.collection.bind('reset', this.addAll);  
		this.collection.fetch();  
		},  

	render: function(){  
		$("#sortable").children().remove();  
		this.addAll();  
	},  
	
	add: function(model){  
		var view = new ContentView({model: model});  
		$("#sortable").append(view.render().el);  
	},  
	
	addAll: function(){  
		this.collection.each(this.add, this);  
	}  
});  
{% endhighlight %}

#####HTML part

{% highlight html linenos=table %}  
<div id="contents">  
	<ul id="sortable">  
	</ul>  
</div>  
{% endhighlight %}

##### template

{% highlight html linenos=table %}
<script type="text/template" id="searchTemplate">  
	<div class="show-content">  
		<label><%- text %></label>  
	</div>  
</script>   
{% endhighlight %}


### Setting up Sortable
***

Now initialise AppView and set up sortable.

{% highlight js linenos=table %}  
    var collection = new ContentCollection();  
    var app = new AppView({collection: collection});  
{% endhighlight %}  

After this the whole page should be able to display models in content. Add models as you like.  


Initialise sortable and trigger  
{% highlight js linenos=table %}  
$(document).ready(function() {  
	$('#sortable').sortable({  
		stop: function (event, ui) {  
			ui.item.trigger('drop', ui.item.index());  
		}  
	});  
});  
{% endhighlight %}  

This event is triggered when sorting has stopped, you can see official document here [event-stop](http://api.jqueryui.com/sortable/#event-stop), it will trigger the item's event 'drop', in this case, it's ContentView.

So we define the event in ContentView  

{% highlight js linenos=table %}  
events: {  
	'drop': 'drop'  
},  

drop: function(event, index){  
	this.$el.trigger('update-sort', [this.model, index]);  
}  
{% endhighlight %}  

Drop function also trigger an envent and passed 2 parameters, one is current dragged item, and the other is the position it dropped.  
 
So in AppView we define 
 
{% highlight js linenos=table %}  
events: {  
	'update-sort': 'updateSort'  
},  

updateSort: function(event, model, position){  
	this.collection.remove(model);  
	var previousIndex = model.get("order");  
	for(previousIndex ; previousIndex <= position ; previousIndex++){  
		this.collection.models[previousIndex].save({"order": previousIndex});  
	}  
	this.collection.add(model,{at: position});  
	this.collection.models[position].save({"order": position});  
	this.render();  
}
{% endhighlight %}  


Once we sort models in collection, sortable will fetch models according to the sequence, so to save the changes we don't have to change collection a lot, only need to change the order in model, and save it in localstorage.
