---
layout: page
category: Java
title: Java Swing JTable 简单使用
tag: [Java,Swing]

---

>主要是想用JTable 来显示用户查询到的数据，不涉及选中事件处理。

----


_通过网上查询到的文章来看，基本上都是使用array来存储JTable里的数据，但是array便于删除，我更希望用链表的方式来存储数据，然后发现可以使用Vector来代替array。_


__变量声明__  
{% highlight java %}  
	private JTable filmTable;  
	private DefaultTableModel filmTableModel;
	
	private Vector filmVectorColName;  
	private Vector filmVectorData;  
{% endhighlight %}  

__初始化__  
	这里我先初始化了table的数据，两个Vector分别记录的是Table的表头和里面的数据。  
{% highlight java %}  
	this.filmVectorColName = new Vector();  
	this.filmVectorData = new Vector();
	
	this.filmVectorColName.addElement("Title");
	this.filmVectorColName.addElement("Year");
	this.filmVectorColName.addElement("Language");
	this.filmVectorColName.addElement("Nationality");
	
	this.filmTableModel = new DefaultTableModel();  
	this.filmTableModel.setDataVector (filmVectorData,filmVectorColName);
	this.filmTable = new JTable(filmTableModel);
{% endhighlight %}  
然后将table添加到Frame中
{% highlight java %}
	final JScrollPane scrollPane_1 = new JScrollPane(filmTable);
	this.filmTable.setFillsViewportHeight(true);
	scrollPane_1.setBounds(400, 30, 350, 70);
	getContentPane().add(scrollPane_1);
{% endhighlight %}

__赋值__  
	我的需求是用户输入查询条件后，再将获得的数据通过JTable显示出来，于是我在查询按钮的触发函数中添加了更改内容的函数
{% highlight java %}
	public void setData(ArrayList list, Vector data){
		for(int i = 0; i < list.size() ; i++){
			Vector vector = new Vector();
			Movie movie = (Movie) list.get(i);
			vector.addElement(movie.getTitle());
			vector.addElement(movie.getYear());
			vector.addElement(movie.getLanguage());
			vector.addElement(movie.getNation());
			data.addElement(vector);
		}
	}
{% endhighlight %}
添加完数据后，更新显示  
{% highlight java %}
	filmTable.invalidate();
	filmTable.updateUI();
{% endhighlight %}



