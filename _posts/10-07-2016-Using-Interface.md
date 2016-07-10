---
layout: post
title: Go Interface
message: "Simple Golang Interface example"
---

When I first started out learning Golang I could not get my head
around what golang's type Interface meant. Golang docs reads, "An interface type specifies a method set called its interface. A variable of interface type can store a value of any type with a method set that is any superset of the interface. Such a type is said to implement the interface. The value of an uninitialized variable of interface type is nil."
<br>
Basically, It means that a type interface hold methods and/or when you want to use method sets just call the interface just like you would a type struct. 
<br>
**This example is about a transaction between two countries would exchange goods and services using dollars.**

{% highlight Go %}

package main

import (
	"fmt"
)

type transaction interface {
	dollars(transaction)
	Name() string
}

type fromChina struct {
	key string
}

func (p fromChina) dollars(t transaction) {
	fmt.Println(p.key, "buys Computers from", t.Name())
}

func (p fromChina) Name() string{
	return p.key
}

type US struct {
	key string
}

func (b US) dollars(t transaction) {
	fmt.Println(b.key, "buys Machines from", t.Name())
}

func (b US) Name() string{
	return b.key	
}

func main(){
	trans1 := &fromChina{"US"}
	trans2 := &US{"China"}
	
	trans1.dollars(trans2)
	trans2.dollars(trans1)
}

{% endhighlight %}

{% highlight Go %}
$go run interface.go
US buying Computers from China
China buying Machines from US

{% endhighlight %}