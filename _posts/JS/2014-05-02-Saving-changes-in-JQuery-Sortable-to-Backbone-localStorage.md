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

First we shoud define Model, Collection, View.

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
	model: Tweet,  
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
	var SearchView = Backbone.View.extend({  
        tagName: 'li',  
        template: _.template($('#searchTemplate').html()),  

        render: function() {  
            this.$el.html( this.template( this.model.toJSON() ) );  
            return this;  
        }  

    });

{% endhighlight %}

#####Define AppView
{% highlight js linenos=table %}  
    var AppView = Backbone.View.extend({  
            el: $("#contents"),  
            
            initialize: function(){  

            _.bindAll(this, 'add', 'addAll');  
            this.collection.bind('add', this.add);  
            this.collection.bind('reset', this.addAll);  
            var collec = this.collection;  
            this.collection.fetch();  
        },  
        
            render: function(){  
            $("#sortable").children().remove();  
            this.addAll();  
        },  
        
        add: function(model){  
            var view = new SearchView({model: model});  
            $("#sortable").append(view.render().el);  
        },  

        addAll: function(){  
            this.collection.each(this.add, this);  
        }  
    });

{% endhighlight %}

#####html file

{% highlight html linenos=table %}  
   <div id="contents">  
        <ul id="sortable">  
        </ul>  
    </div>  
{% endhighlight %}

##### template

{% highlight html linenos=table %}
    <script type="text/template" id="searchTemplate">  
        <div class="search-result">  
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

initialise sortable and trigger  
{% highlight js linenos=table %}  
    $(document).ready(function() {  
        $('#sortable').sortable({  
            stop: function (event, ui) {  
                ui.item.trigger('drop', ui.item.index());  
            }  
        });  
    });  
{% endhighlight %}  

then in SearchView define the events  

{% highlight js linenos=table %}  
        events: {  
            'drop': 'drop'  
        },  
        
        drop: function(event, index){  
            this.$el.trigger('update-sort', [this.model, index]);  
        }  
{% endhighlight %}  

and in AppView define event  
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
