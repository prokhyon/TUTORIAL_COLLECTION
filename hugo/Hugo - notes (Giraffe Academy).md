# Hugo tutorial - Giraffe Academy

[video tutorial](https://www.youtube.com/watch?v=qtIqKaDlqXo&index=1&list=PLLAZ4kZ9dFpOnyRlyS-liKL5ReHDcj4G3)

Relevant themes:

https://github.com/giraffeacademy/ga-hugo-theme

https://themes.gohugo.io/kraiklyn/

https://themes.gohugo.io/blackburn/

https://themes.gohugo.io/hyde/

https://themes.gohugo.io/hyde-hyde/

https://themes.gohugo.io/docuapi/

https://themes.gohugo.io/purehugo/

https://themes.gohugo.io/strange-case/

https://themes.gohugo.io/redlounge/

https://themes.gohugo.io/hugo-code-editor-theme/

https://themes.gohugo.io/hugo-uno/

https://themes.gohugo.io/hurock/

https://themes.gohugo.io/twentyfourteen/

https://themes.gohugo.io/greyshade/




## 4. site creation

```
hugo new site <sitename>
```

## 5. themes

https://themes.gohugo.io

theme.toml

```
config.toml: theme = "<theme-name>"
```
```
hugo server
```

## 6. contents
```
hugo new path/filename

hugo server -D 	# draftokat is kiszolgál

```
hugo content types: single pages vs. list pages

generated list template for selected directory

custom list pages in subdirectories: _index.hu


## 7. front matter
yaml (\-\-\-), toml (+++), json

variables

kérdések:
json-höz separator?


## 8. archetype

content generation: default.md, <dirname>.md

kérdések: 
ha több dirname-es folder van?
ha alkönyvtár alkönyvtárában hozzuk létre és csak egyikre van archetype?


## 9. shortcodes

insert predefined chunks of html -> markdown files

default shortcodes = predefined HTML

```
{{< shortcode_name parameter1 >}}
```


## 10. taxonomies

taxonomy: tags and categories

generated list template for selected taxonomy

custom taxonomy: config.toml

```
[taxonomies]
  tag = "tags"
  category = "categories"
  label = "labels"
```

kérdések:
mi van ha egy kategória név megegyezik egy tag névvel?
a kategória tulajdonképpen csak egy tag, vagy hieararchikus is?


## 11. templates

themes/<theme>/layouts: _default, partials

themes/<theme>/layouts/_default: list.html, single.html  # felülírja a témáét

themes/<theme>/layouts/partials: 404.html

layouts/_default # felülírja a theme-jét

list and single templates -> list and single pages

list.html:

```
{{ partial "header" (dict "Kind" .Kind "Template" "List") }}
{{ .Content }}
{{ range .Pages }} 
  {{ .Title }}
  {{ dateformat "Monday, jan2, 2006" .Date }}
  {{ if .Params.tags }}
  {{ if .Params.categories }}
  {{ if .Params.moods }}
  {{ .Summary }}
{{ end }}
{{ partial "footer" . }}
```

## 12. list templates

list pages: automatically generated or content/<dirname>/_index.md

custom list template: layouts/_default/list.html # felülírja a témáét

variable: ```{{ .Content }} {{ .Title }} {{ .URL }}```

function: ```{{ range .Pages }} ... {{ end }}```


## 13. single templates

content/content.md

custom single template: layouts/_default/single.html # felülírja a témáét


## 14. homepage templates

technikailag list page, ezért alapértelmezettként list template-et használ

layouts/index.html


## 15. section templates

section: a content-ben levő könyvtárak nevei

layouts/<dirname> # a content/<dirname> alapján

a template a content könyvtárban levő fájlok megjelenítésére vonatkozik


## 16. base templates and blocks

kódismétlés helyett egy magasabb szintű template

layouts/_default/baseof.html:

```
<html> ... {{ block "main" . }} ... {{ end }} ... </html>
```
layouts/_default/single.html:
```
{{ define "main" }} This is the single template {{ end }}
```
layouts/_default/list.html:
```
{{ define "main" }} This is the list template {{ end }}
```
nem kötelező felülírni a single.html/list.html templatek-ben


## 17. variables

https://gohugo.io/variables

layouts könyvtárból elérhetők

content könyvtárból nem elérhetők # csak a templates vagy layouts alatt lehet használni

Front matter variables:
```
{{ .Title }}
{{ .Date }}
{{ .URL }}
```
Custom front matter variables: 
```
---
var: "myValue"
---

{{ .Params.myVar }}
```
Custom variables:
```
{{ $myVarName1 := "aString" }}
{{ $myVarName2 := true }}
{{ $myVarName3 := 3 }}
{{ $myVarname1 }}
<h1 style="color: {{ .Params.color }};">title</h1>
```


## 18. functions

https://gohugo.io/functions

```
{{ funcName param1 param2 }}
{{ truncate 10 "This is a really long string" }}
{{ add 1 5 }}
{{ sub 1 5 }}
{{ singularize "dogs" }}
{{ range .Pages}} ... {{ end }}
{{ range .Pages}}
	{{ .Title }}  # in context
{{ end }}
```

## 19. conditionals

eq, lt, le, gt, ge, not, and, or

```
{{ $var1 := "dog" }}
{{ $var2 := "cat" }}
{{ $var3 := 3 }}
{{ $var4 := 4 }}
{{ if eq $var1 $var2 }}...{{ else if <cond>}}...{{ else }}...{{ end }}
{{ if not (eq $var1 $var2) }}
{{ if and (eq $var1 $var2) (lt $var3 $var4) }}...{{ end }}
{{ $title := .Title}}
{{ range .Site.Pages}}
	<li><a href="{{ .URL }}" style="{{ if eq .Title $title}} background-ccolor:yellow;{{end}}" >{{ .Title }}</a></li> # scope change: current page vs. contect variable
{{ end }}
```


## 20. data

json, yaml, toml fájlok

pl. states.json:
```
{{ range .Site.Data.states }}
	{{ .name }} : {{ .capital }}<br/>
{{ end }}
```

kérdés: mi van akkor, ha ugyanazzal a névvel, de eltérő kiterjesztéssel vannak a fájlok?


## 21. partial templates

html files

pl. layouts/partials/header.html

```
{{ .Title}} : {{ .Date }}
{{ partial "header" . }} # . is the scope of the current file
```
```
{{ .myTitle }} : {{ .myDate }}
{{ partial "header" (dict "myTitle" "myCustomTitle" "myDate" "myCustomDate") }} # custom dictionary
```


## 22. shortcode templates

valójában partial template-ek

olyan kódot illeszt be, ami máshol van definiálva

markdown fájlokba tehető a HTML kód helyett

single tag shortcode vs. doublely tagged

single tag shortcode:

pl. layouts/shortcodes/myshortcode.html

html: 
```
This is the shortcode text.
```
md: 
```
{{< myshortcode >}}
```
escaping parameter: 

md: 
```
{{ .Get "color" }}
```
html: 
```
<p style="color:{{ .Get `color` }};">This is the shortcode text.</p>
```
with named parameter:

md: 
```
{{< myshortcode color="blue" >}}
```
positional parameter:

html: 
```
<p style="color:{{ .Get 0 }};">This is the shortcode text.</p>
```
md: 
```
{{< myshortcode "blue" >}}
```
doublely tagged:

md: 
```
{{< myshortcode >}}
	Text inside shortcode text
{{< /myshortcode >}}
```
html: 
```
<p style="background-color:yellow;">{{ .Inner }}</p>
```
doublely tagged + markdown syntax:

md:
```
{{% myshortcode %}}
	Text inside \*\*shortcode\*\* text
{{% /myshortcode %}}
```


## 23. building and hosting

```
hugo server -D
hugo	# public folder is generated with static HTML files
```
public könyvtár törlése

kérdés: mi van ha content/tags könyvtár van és tag-eket használunk, a public folderben nem ütközik?

