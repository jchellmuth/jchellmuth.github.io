Publication highlights:
publication highlights are rendered in a table. 
Vertical table borders were removed by changing settings in the `_base.scss` file
`border-left: none; and border-right: none;`  
and the table background was set to $background color

SEO:
to exclude certain pages from search engines, I am doing two things 
1. Exclude from sitemap file (which helps google to crawl the page).
This is done by setting `sitemap: false` in the YAML front matter
2. Use the robots meta tag
General information can be found here:
https://developers.google.com/search/reference/robots_meta_tag
The Jekyll solution I implemented is from this blog post:
http://longqian.me/2017/02/12/jekyll-robots-configuration/
Briefly, this was added to the `_includes/head.html` file:
{% if page.robots %}
<meta name="robots" content="{{page.robots}}" />
{% endif %}
Then, the following tag is added to pages that you don't want to appear in search engine results:
robots: noindex
This will lead to the following robots meta tag in the final html:

For now, I am excluding my CV and cooking page incl. all recipe posts from search engines.

Notes (from Jekyll website):
Kramdown is the default Markdown renderer for Jekyll.
By default, Jekyll uses the GitHub Flavored Markdown (GFM) processor for Kramdown.

Parsers that do not support tables without headers
... GitHub Flavoured Markdown ...

Parsers that do support tables without headers: 
... Kramdown: A parser in Ruby ...
... Pandoc ... 


Notes on tables:
- kramdown supports tables without headers (which I like) and is used by jekyll.
- tables in kramdown / jekyll require a preceeding empty line (this line doesn't have to include a line break).
- it seems that there is no way to make markdown previews in atom use kramdown. Thus, tables will not render properly here.
- To get a better preview of tables, I usually add a line-break (2x space) after each row (however, this is not required to Jekyll).
