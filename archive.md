---
layout: page
title: Archive
permalink: /archive/
---
<!--This page lists all posts (not pages) organized by date.-->

<div class="post-list">
    
        {% for post in site.posts %}
            <li>
                <a href="{{ post.url }}">
                    {{ post.title }}
                </a>
                <time>{{ post.date | date: '%B %d, %Y' }}</time>
            </li>
        {% endfor %}
    
</div> <!-- .post-list -->