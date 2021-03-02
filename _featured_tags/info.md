---
# Featured tags need to have either the `list` or `grid` layout (PRO only).
layout: list

# The title of the tag's page.
title: info

# The name of the tag, used in a post's front matter (e.g. tags: [<slug>]).
slug: info

# (Optional) Write a short (~150 characters) description of this featured tag.

# (Optional) You can disable grouping posts by date.
# no_groups: true

# Exclude this example category from the sitemap.
# DON'T USE THIS SETTING IN YOUR CATEGORIES!
sitemap: false
---
<div class="tags-expo-list">
  {% for tag in site.tags %}
  <a href="/posts/tag-{{ tag[0] | slugify }}" class="post-tag">{{ tag[0] }}</a>
  {% endfor %}
</div>
<hr/>
