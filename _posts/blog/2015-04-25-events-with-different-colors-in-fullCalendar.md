---
layout: post
title: "Render events with different colors in fullCalendar"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-04-25T10:16:00+0530
comments: true
---

FullCalander is a jquery plugin to implement calendar and events in calendar.

Basic usage:
Add a div with id 'calendar' in your html then,

{% highlight js %}
$('#calendar').fullCalendar({
       header: {
              left: 'prev,next today',
              center: 'title',
              right: 'month,agendaWeek,agendaDay'
              },
       defaultDate: new Date(),
       slotDuration: '00:30:00',
       editable: true,
       selectable: true,
       selectHelper: true,
       height: 'auto',
       events: [
                {
                   title: 'All Day Event',
                   start: '2015-02-01'
                },
                {
                   title: 'Lunch',
                   start: '2015-02-12T12:00:00'
                },
                {
                   title: 'Meeting',
                   start: '2015-02-12T14:30:00'
                },
                {
                   title: 'Dinner',
                   start: '2015-02-12T20:00:00'
                },
                {
                   title: 'Birthday Party',
                   start: '2015-02-13T07:00:00'
                }
               ]
})
{% endhighlight %}

If you want different colors for events then eventRender call back will be helpful. 
Assign a variable color to event object and then use that to change color of event.

Example:

{% highlight js %}
event = {
         title: 'Dinner',
         start: '2015-02-12T20:00:00',
         end: '2015-02-12T21:00:00',
         color: '#ff00ac'
       }
{% endhighlight %}


Now use this attribute to change color of event.

{% highlight js %}
eventRender: function (event, element, view) {
   if (event.color) {
       element.css('background-color', event.color)
   }
}
{% endhighlight %}
