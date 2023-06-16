---
title: "Tailwind CSS: Buttons"
datePublished: Thu Sep 16 2021 12:52:48 GMT+0000 (Coordinated Universal Time)
cuid: cktmxnx4m03pzz7s1d88p42aw
slug: tailwind-css-buttons
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1631796695032/Mx8LYC9omj.png
tags: css, tailwind-css, tailwind-css-tutorial

---

This week was my first time exploring Tailwind CSS while building buttons components on React for [devChallenges](https://devchallenges.io/challenges/ohgVTyJCbm5OZyTB2gNY) project challenges. Most people have been talking good about Tailwind CSS being an efficient way to style components. Here are some resources that have been of help for the components of the button:-
- [installing Tailwind CSS on create-react-app](https://tailwindcss.com/docs/guides/create-react-app) i.e setting up create-react-app, installing tailwind, setting up craco on package.json, setting up Tailwind on the CSS file
- [Tailwind css Docs](https://tailwindcss.com/docs)
- [Tailwind CSS V1 Docs](https://v1.tailwindcss.com/docs/installation) 
- [7 Tailwind Buttons](https://www.ordinarycoders.com/blog/article/7-tailwindcss-buttons) this has different types of buttons styled using Tailwind CSS i.e
> basic primary buttons: `hover`
```
<a href="#" class="bg-white hover:bg-gray-200 text-black text-center py-2 px-4 rounded">
  Button
</a>
```
 outlined secondary buttons: `border`
```
<a href="#" class="bg-transparent border border-black text-black hover:bg-black hover:text-white text-center py-2 px-4 rounded">
  Button
</a>
```
 light buttons: `background-color`
```
<a href="#" class="bg-red-200 hover:bg-red-500 hover:text-white text-red-500 text-center py-2 px-4 rounded">
  Button
</a>
```
 buttons with icons: `inline-flex and items-center`
```
<a href="#" class="bg-red-500 hover:bg-red-700 text-white py-2 px-4 rounded inline-flex items-center">
  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" />
  </svg>
  Button
</a>
```
 rounded buttons: `rounded-full`
```
<a href="#" class="bg-blue-500 hover:bg-blue-700 text-white text-center py-2 px-4 rounded-full">
  Button
</a>
```
 circular buttons: `rounded-full, inline-flex and items-center`
```
<a href="#" class="bg-green-500 hover:bg-green-700 text-white text-center py-2 px-4 rounded-full h-14 w-14 inline-flex items-center">
  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" />
  </svg>
</a>
```
 3D buttons: `border-b-4`
```
<a href="#" class="bg-pink-500 hover:bg-pink-400 border-b-4 border-pink-700 hover:border-pink-500 text-white text-white text-center py-2 px-4 rounded">
  Button
</a>
```
Thank for reading through my article 