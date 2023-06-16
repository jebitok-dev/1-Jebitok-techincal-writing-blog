---
title: "Code First Girls Summer Course Experience"
datePublished: Mon Oct 26 2020 18:09:17 GMT+0000 (Coordinated Universal Time)
cuid: ckgv6j8z70048bxs173r0ev5i
slug: code-first-girls-summer-course-experience
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1603996255203/hcYLhzavO.png
tags: css, web-development

---

Back in late August, I came across this opportunity for [Code First Girls](https://codefirstgirls.org.uk/) Summer programs and I thought I should apply for it. I got accepted for the Introduction to Web Development, despite the fact that I have learned JavaScript, React, and on I think I still have a great gap when it comes to CSS and bootstrap that I still have to practice and learn more around and thought this program would be a great fit for me. 

Over the program, we had 8 weeks of learning one day per week ( every Wednesday for our group). My goal was to take part in all sessions and to be able to work on a group project that makes part of the best 3 at the end. 

Here is our weekly program from week 1 up to week 7. The program was project-based we were building a portfolio project together using [codesandbox](https://codesandbox.io/). In the first part of the sessions, the instructors would take us through the learning resources then we normally used the knowledge we gain to implement some sections of the portfolio sections which were laid out as user stories using various languages like the first three weeks we started with basic HTML5  and CSS3, the next two weeks on Bootstrap and last weeks on JavaScript and finishing our projects on the side.  

The CSS part is memorable to me cause I'm not so good in CSS I remember I would always rely on my designer friend to fix my parts in projects we working on together but over this program, I was able to grasp some CSS properties like the:-

*Box Model properties*

You can use padding (transparent space), border and margin to style the elements of HTML i.e text/images put in HTML tags.

```
padding: 10px;
border: 1px;
margin: 25px;
``` 
```
padding-top: 10px
padding-bottom: 10px;
padding-left: 5px+
```
*Flow & positioning*

*Block elements* includes tags like *div, p,  li, ul, and h1* among others whereas *img, a* tags among others are *inline elements*.

*Mobile First*

When developing a website you're advised to start focusing on its mobile responsiveness before that of larger screens since most users use mobile instead of desktop. An example of a media query is this:-
```
@media only screen and (min-width: 768px){
body {
background-color: blue;
   }
}

@media only screen and (max-width: 992px) {
body {
background-color: pink;
   }
}
```
*Flexbox/Flexible Box Layout*

It's an efficient way to layout/align and distribute space among items in a container using flexible widths and heights to achieve a responsive design system by scaling vertically, aligning horizontally, and re-ordering elements.

- Have a flex container as the parent div

*Flex Items*

horizontally aligned along the main-axis i.e 
  - main end and the main start of the y-axis 
  - cross start and cross-end

```
.parent {
border: 1px solid black;
display: flex;
flex-direction: column;
}
```
> Difference between Justify-Content and  Align Items

*Justify-content*

It gives space, padding, and margin equal space on left & right horizontally along the main axis. 

```
justify-content: flex-end;
justify-content: space-between;
justify-content: stretch
```
*Align-items*

It gives space vertically on the cross axis.
```
align-items: center
align-items: end
align-items: space-between
align-items: stretch
```
There are other flex-items properties apart from justify-content and align-items like *flex-wrap, flex-flow, align-content, flex-shrink, flex-direction, and display: flex*;

After week 3 we started converting our project from basic HTML to using bootstrap to add responsiveness following weeks we used JavaScript to add functionality on parts like the forms of the portfolio. At this stage, we also got into groups and started designing and implementing our final group projects. Here is our final project my group we were working on [women in technology](https://womenintechn.netlify.app/) website

 Glad that I've made to the end of the CFG course having gained some skills, build a network from the community, and will probably sign up for another short course as well as recommend to a friend.

> Thanks for reading my article you can read the previous ones as well!


