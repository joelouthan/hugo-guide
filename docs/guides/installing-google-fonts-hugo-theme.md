---
layout: default
title: "How to install Google Fonts into a hugo theme"
---
{:.no_toc}

This is a step-by-step guide on how to install and configure [Google Fonts](https://fonts.google.com). The example in this guide is within the [Cupper theme](https://github.com/zwbetz-gh/cupper-hugo-theme) for [hugo server](https://gohugo.io).

Let's get started.

{:toc}

# Tested System

|**Software**   |**Version**   |
|---|---|
|**hugo**   |0.59.0   |
|**Go**   |1.12   |
|**PHP**   |5.6   |
|**ruby**   |2.6.2   |
|**node**   |v10.22.0   |
|**npm**   |v6.14.6   |
|**Cupper**   |[Source](https://github.com/zwbetz-gh/cupper-hugo-theme)   |
|**hosting**   |[netlify](https://www.netlify.com)/build 3.3.5  |

# Prerequisites

1. hugo server is running
2. Site is live and running
3. For this example, we are using the [Cupper theme](https://github.com/zwbetz-gh/cupper-hugo-theme)

# Instructions

1\. Edit the head.html file and place the following snippet within the head tag. (Note: For our example, head.html was located in the themes/cupper/layout/partials directory)

{% raw %}
```html
<head>
   {{ partial "google-fonts" . }}
</head>
```
{% endraw %}
> **TIP**: Do not place this snippet within the meta tag.

2\. Edit the config.toml file and place the google_fonts line underneath the params tag. (Note: this file is located within the webroot directory):

{% raw %}
```toml
   [params]
   google_fonts = [
     ["Cinzel", "100, 200, 300, 400, 500, 600, 700, 800, 900"]
   ]
```
{% endraw %}

> **NOTE**: 100-900 are the various weights–100 being the thinnest and 900 being the boldest.

3\. Create the file: webroot/themes/theme-name/layouts/partials/google-fonts.html and add the following to this file:

{% raw %}
```html
   {{ if .Site.Params.google_fonts }}
     {{ $fonts := slice }}
     {{ range .Site.Params.google_fonts }}
       {{ $family := replace (index (.)  0) " " "+" }}
       {{ $weights := replace (index (.) 1) " " "" }}
       {{ $string := print $family ":" $weights }}
       {{ $fonts = $fonts | append $string }}
     {{ end }}
     {{ $url_part := (delimit $fonts "|") | safeHTMLAttr }}
     <link {{ printf "href=\"//fonts.googleapis.com/css?family=%s\"" $url_part | safeHTMLAttr }} rel="stylesheet">
   {{ else}}
     <!-- specify a default in case custom config not present -->
     <link href="//fonts.googleapis.com/css?family=Roboto:300,400,700" rel="stylesheet">
   {{ end }}
```
{% endraw %}

4\. Edit your site's css file. (For this example, I wanted to alter the library-desc property.)

```css
   .library-desc {
       font-size: 0.60rem;
       font-weight: 100;
       font-family: "Cinzel", Times, serif;
       margin-top: 0.5rem;
       margin-left: auto;
       margin-right: auto;
       max-width: 25rem;
   }
```

5\. **For local hugo server**: Stop and restart hugo server

6\. Deploy into production

# Verification

Once your site has been deployed, check to see if the fonts have been rendered correctly.

For example:

![image-20200619192818033](image-20200619192818033-9187937.png)

# References

[Google Fonts via config with Hugo](https://gist.github.com/jeremybise/a6afea2d4c7f9044180ffeb663a617cf)

   