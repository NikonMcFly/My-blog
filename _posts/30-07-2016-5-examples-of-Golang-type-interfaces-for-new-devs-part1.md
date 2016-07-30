---
layout: post
title: 5 type interface examples for Golang newbies part 1
message: "Great details on understanding Golang interface"
---
In this blog post I will be ripping apart the golang interface types and going step by step on understanding what each part means with 5 examples.
<br>Lets get started!
<br>

#### Method set 
An interface holds method sets. A method means that when a function is called, the method will act.

![alt tag](/public/interface-type.png)

Example, I exercise. I can either sprint or jog.
{% highlight Go %}
type exercise interface {
	Sprint()
	Jog()
}
{% endhighlight %}
Sprint and jog are exercise's interface. 

#### Method set with parameters
I can also put other parameters in sprint and jog or sprint methods. In the spec of the language below it states...

![alt tag](/public/interface-unique-name.png)
This means that an interface takes Read, Write, and Close method. Read and Write returns a Boolean.
Example, if I sprint, I am sprinting a short distance. If I jog, I am jogging a long distance and I want to return a string.

{% highlight Go %}
type exercise interface {
	Sprint(b []byte) string
	Jog(b []byte) string
}
{% endhighlight %}


#### Implement a interface with func
In order to use these methods you have to implement them in a function to be called to do some sort of task. Next in the spec...

![alt tag](/public/interface-func-and-struct.png)

"Where T stands for either S1 or S2..." means in our example below in order to implement both "S1" and "S2" we have to connect them or share. We will use a struct. A struct just holds variables.

Example 1

{% highlight Go %}
type human struct{
	Name string
}

type exercise interface {
	Sprint()
	Jog()
}

func (e human) Sprint(){
	
}
func (e human) Jog(){
	
}
{% endhighlight %}

Example 2

{% highlight Go %}
type human struct{
	Name string
	age int
}

type exercise interface {
	Sprint(b []byte) string
	Jog(b []byte) string
}

func (e human) Sprint(b []byte) string{
	
}
func (e human) Jog(b []byte) string{
	
}
{% endhighlight %}

#### Embedding interface

Embedding interface is creating an interface and putting that interface into another interface you want to create. The specs states this well.

![alt tag](/public/interface-embedding.png)

****We can not access methods inside of interfaces into another interface****
<br>
Say we want to stretch after our workout. We would embed exercise interface and stretch interface into workout interface.
{% highlight Go %}
type exercise interface {
	Sprint(b []byte) string
	Jog(b []byte) string
}
type stretch interface {
	Touchtoes()
}

type workout interface {
	exercise
	stretch
}

{% endhighlight %}

Also if you understand everything above, you should be able to understand this piece of the docs for itself. One interface can not embed itself or any interface type that embeds another.

![alt tag](/public/interface-illegal.png)


## 2 type interface examples 

Example 1. Lets finish our human working out.

{% highlight Go %}
import(
	"fmt"
)

type human struct{
	Name 	string
	Age 	string
}

type exercise interface {
	Sprint() string
	Jog() string
}

func (h human) Sprint() string{
	return h.Name + " ran so fast that it took him 4 seconds and he is only "
}

func (h human) Jog() string{
	return h.Name + " jog so fast that it took him 12 minutes! He is " + h.Age + " years old."
}

func main(){
	steve := human{Name: "Steve", Age: "21"}
	fmt.Println(steve.Sprint())
	fmt.Println(steve.Jog())
}
{% endhighlight %}

Example 2. Is the plane in the air or not?

{% highlight Go %}

import(
	"fmt"
)

type plane struct {

}

type fly interface {
	air(up bool) string
}

func (plane) air(up bool) string {	
	if up == true{
		return "Plane is up in the air"
	}
	return "Plane is in the hangar"
}

func main() {
	p := plane{}
	plane := p.air(true)
	fmt.Println(plane)

	plane = p.air(false)
	fmt.Println(plane)
}
{% endhighlight %}

This will be part one of a part two series on interfaces in golang.