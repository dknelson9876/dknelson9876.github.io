Title: Sphenodon, My Custom Pelican Theme
Date: 2023-07-10
Tags: pelican
Authors: dknelson
Summary: I'd prefer this website had a darkmode. The best looking theme I found is broken, so time to recreate myself

With nothing against the default theme that Pelican uses, I personally prefer dark themes. After trying to look around the internet for a suitable theme, I really like the look of the [Astrochelys theme](https://out-of-cheese-error.netlify.app/astrochelys). Unfortunately, when I clone the theme directly, I run into some issues that I am struggling to resolve. Especially with figuring out how to install the expected `pelican-toc` plugin. So, I'm going to do my best to mash together Astrochelys and the default theme into my own theme, that, in keeping with the apparent theme of naming after creatures, I'm calling Sphenodon (see [Github repo](https://github.com/dknelson9876/sphenodon)).

Most of this work consisted of digging out of the depths of my memory how HTML and CSS work, then finding the right classes to add to the right tags to adopt Astrochelys's theming. There's a lot of things I'm leaving out as well, as I don't care about analytics or RSS feeds.

I especially like the way that the metadata for the article (date, category, tags) is moved over to the margin, inside a `<div class="marginnote">`. I may go back and change that style a little, as I'm wondering if that font is too small. Unfortunately, it looks like this also means it disappears on mobile, so I'll have to do something about that. There is already some styling that is width dependent for the left sidebar, so hopefully that will be a guide.


<details>
    <summary>Here's a test of code blocks with the code for the metadata:</summary>

```html

    <div class="marginnote">
        <hr>
        <div class="article-information">
            <div class="article-information-heading uppercase">Published</div>
            <time class="published" datetime="{{ article.date.isoformat() }}">
                {{ article.locale_date }}
            </time>
            {% if article.modified %}
            <time class="modified" datetime="{{ article.modified.isoformat() }}">
                {{ article.locale_modified }}
            </time>
            {% endif %}
            {% if article.category %}
            <div class="category">
                Category: <a href="{{ SITEURL }}/{{ article.category.url }}">{{ article.category }}</a>
            </div>
            {% endif %}
            {% if article.tags %}
            <div class="tags">
                Tags:
                {% for tag in article.tags %}
                <a href="{{ SITEURL }}/{{ tag.url }}">{{ tag }}</a>
                {% endfor %}
            </div>
            {% endif %}
        </div>
        <hr>
    </div>
```
</details>

Some of the most notable differences in the organization of the site is the removal of the individual 'period-archive', 'tag', and 'category' pages in favor of only having the 'archive', 'tags', and 'categories' pages, which have anchors within the page for the individual years/tags/categories. With my eventual intention to have (at least) a search function for this site, this is far from over.

I've noticed that I miss the way the default theme shows the first bit of the most recent article, not just the summary, so I should do something about that.