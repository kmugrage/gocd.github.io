---
layout: default
title:  Events
title_tag: GoCD Events & Learning Sessions
meta_tag_description: GoCD events and learning sessions
meta_tag_keywords: go events, learning session, continuous delivery, open source, gocd, go cd
---

#### Upcoming Events

{% for event in site.data.events %}
- <a href="{{ event.url }}">{{ event.title }}</a> - {{ event.date }} - {{ event.description }}
{% endfor %}
