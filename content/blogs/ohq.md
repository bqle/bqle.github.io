---
title: "Office Hours Queue (OHQ)"

date: 2022-10-06T09:10:08-04:00
draft: false
image: /blogs/ohq.png
---
*My experience working as a backend developer and ultimately team lead at OHQ.*

<!--more-->
A significant part of my time at Penn has been dedicated towards OHQ - a tool that intuitively streamlines office hours for 7k+ students in the Penn community. I regularly work with the Django REST framework, Django ORM, auto-docs, sockets, and CI/CD tools.

Essentially, the product allows students to join a virtual queue and the TA will help students one-by-one in the order in which they joined the queue. In addition to this core purpose, we provide live-analytics on the expected wait time for the queues and quality-of-life features that help TAs organize their OH (like an optional bell reminder if they are helping one student for too long).
### Pins
The first feature I implemented for OHQ as my onboard project was giving TAs the ability to enforce pins in their office hours. When classes moved back to in-person and so did office hours, our product - which was designed for the online environment - did not allow TAs to enforce only in-person office hours. With the help of pins, a TA can write a secret pin that only in-person students will be able to see, and require the secret pin in order for a student to join the queue.

Unsurprisingly, this feature can be circumvented if students share the passcode with each other, but ultimately we assume the users are using it in good faith. It also serve as an implicit way to let students that this queue expects students to be in person. 

Through this project, I gained familiarity with the Models, Serializers, and Viewsets classes in Django and their relationship. I also touched some of the permission and auto-docs file for documentation.
### Calendar
After gaining more familiarity with the codebase, I took on a project that allows TAs to schedule, display, change events right in the app. The main goal of this feature is to help concentrate all the office hour schedules for all their classes into one place. 

This was the first project where I worked with an external package `django-scheduler` to help with the management of event objects and their storage. Me and a teammate also planned what sort of APIs we want to support, so that it the easiest for the frontend team to make use of this API.
Thinking about the relationship between Event (like an OH) and Occurrence (an occurrence of an OH event) also requires much thought when one consider recurring events. Also questions like whether we should lazily create the Occurrence objects in our database or not also came to mind. Also, what happens when a TA cancel/change an occurrence of a recurring event? How will we store and retrieve this change?

Through this project, I became even more confident with my understanding of Django and my ability to use external Django packages. Applying the lazy creation of Occurrence objects was the highlight of this project for me. As a freshman, it showed me how much more there is to learn about the optimization of servers & databases. 

### File upload system

A project I am currently working on grants users the ability to share files directly on the app. For this feature, I worked with new Django concepts like the MultipartParser - for working with request bodies that are of multipart - and the FileField - Django's way of associating files to an object in the ORM.

While I have completed the logic for the file uploader (the viewset, serializers, permissions), my next step will be to integrate it with our team's S3 bucket. I do not expect the integration to take too long, but I will have to learn about it through our team's other codebases, and look forward to learning more about boto3. 

