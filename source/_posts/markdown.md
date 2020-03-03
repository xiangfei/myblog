---
title: markdown
date: 2020-03-02 16:22:01
tags: markdown
author: 相飞
comments:
- true
categories:
- markdown


---


### markdown 画图




```mermaid
graph TD
A[Client] --> B[Load Balancer]
B --> C[Server01]
B --> D[Server02]

```

{% mermaid %}
 graph LR
      A --- B
      B-->C[fa:fa-ban forbidden]
      B-->D(fa:fa-spinner);

{% endmermaid %}
