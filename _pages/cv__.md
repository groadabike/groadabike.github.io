---
layout: none
permalink: /cv/
title: CV
nav: false
nav_order: 4
target: _blank
---
<!doctype html>
<html lang="{{ site.lang | default: 'en' }}">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="refresh" content="0; url={{ '/assets/pdf/cv.pdf' | relative_url }}" />
    <link rel="canonical" href="{{ '/assets/pdf/cv.pdf' | absolute_url }}" />
    <script>
      window.location.replace({{ '/assets/pdf/cv.pdf' | relative_url | jsonify }});
    </script>
  </head>
  <body>
    <p>Redirecting to <a href="{{ '/assets/pdf/cv.pdf' | relative_url }}">the CV PDF</a>.</p>
  </body>
</html>
