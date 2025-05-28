---
title: Saving changes in jQuery Sortable to Backbone localStorage
date: 2014-05-02
category: JS
tags:
  - JS
toc: true
---



> The goal is to using jQuery Sortable to show models stored in localStorage and saving the sequence after changing.

### Getting started

***

First is to define Model, Collection, View.

##### Define Model
```javascript
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
```

##### Define Collection
```javascript
var ContentCollection = Backbone.Collection.extend({  
	model: Content,  
	localStorage: new Backbone.LocalStorage("contentList"),  
	nextOrder: function(){  
		if (!this.length) return 0;  
		return this.last().get('order') + 1;  
  },  

  comparator: function(model) {  
    return model.get('order');  
  },  

  reOrder: function(){  
    this.each(function(model, index) {
        if (model.get('order') !== index) {
            model.save({ "order": index });
        }
    });  
  }
});
```
_Note: the comparator is to sort models in collection_

##### Define View
```javascript
var ContentView = Backbone.View.extend({  
        tagName: 'li',  
        template: _.template($('#searchTemplate').html()),
        render: function() {  
          this.$el.html( this.template( this.model.toJSON() ) );  
          return this;  
    }  

});
```

This is the view shows each single model

##### Define AppView
```javascript
var AppView = Backbone.View.extend({  
	el: $("#contents"),  
	initialize: function(){  
    _.bindAll(this, 'add', 'addAll');  
    this.collection.bind('add', this.add);  
    this.collection.bind('reset', this.addAll);
    this.listenTo(collection, 'sort', this.render);
    this.collection.fetch();  
	},  

  render: function(){  
    $("#sortable").children().remove();  
    this.addAll();
    return this;
  },  

  add: function(model){  
    var view = new ContentView({model: model});  
    $("#sortable").append(view.render().el);  
  },  

  addAll: function(){  
    this.collection.each(this.add, this);  
  }  
});  
```


##### HTML part

```html
<script type="text/template" id="searchTemplate">  
	<div class="show-content">  
		<label><%- text %></label>  
	</div>  
</script>
```


### Setting up Sortable
***

Now initialise AppView and set up sortable.

```javascript
var collection = new ContentCollection();  
var app = new AppView({collection: collection}); 
```

After this the whole page should be able to display models in content. Add models as you like.  

Initialise sortable and trigger  

```javascript
$(document).ready(function() {  
	$('#sortable').sortable({  
		stop: function (event, ui) {  
			ui.item.trigger('drop', ui.item.index());  
		}  
	});  
}); 
```

This event is triggered when sorting has stopped, you can see official document here [event-stop](http://api.jqueryui.com/sortable/#event-stop), it will trigger the item's event 'drop'. In this case, it's ContentView, add the event in ContentView  

```javascript
events: {  
	'drop': 'drop'  
},  

drop: function(event, index){  
	this.$el.trigger('update-sort', [this.model, index]);  
}  
```

Drop function also triggers an envent and passed 2 parameters, one is current dragged item, and the other is the position it dropped. In AppView we define 

```javascript
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
```

