---
title: Projects - Flynn Cao
display: Projects
subtitle: List of projects that I am proud of
description: List of projects that I am proud of
plum: false
projects:
  Upcoming:
    - name: 'vite-h2o-plus'
      link: 'https://twitter.com/flynnchao99/'
      desc: 'Starter template with custom theme based on Vite, Vue3, TypeScript and Element Plus'
      icon: 'i-icon-park-twotone-water-level'

  StarterTemplate:
    - name: 'vite-h2o'
      link: 'https://github.com/flynncao/vite-h2o'
      desc: 'Starer template based on Vite, Vue3 and Typescript.'
      icon: 'i-icon-park-twotone-water'
			
  Video:
    - name: 'h265web-vue-demo'
      link: 'https://github.com/unjs/h265web_vue_demo'
      desc: 'Implemented the h265 player by VueJS using local nginx service to play.'
      icon: 'i-ic-sharp-slow-motion-video'
  Music:
    - name: 'Aero Player (Old)'
      link: 'https://github.com/FlynnCao/AeroPlayer'
      desc: ' Cute and Lovely HTML5 Music Player.'
      icon: 'i-akar-icons-music-album-fill'
---


<ListProjects :projects="frontmatter.projects"/>

<StarsRanking/>
