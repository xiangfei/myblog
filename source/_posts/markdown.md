---
title: markdown (mermaid)
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
{% mermaid %}
graph TD
A[Client] --> B[Load Balancer]
B --> C[Server01]
B --> D[Server02]
{% endmermaid %}
```

{% mermaid %}
graph TD
A[Client] --> B[Load Balancer]
B --> C[Server01]
B --> D[Server02]

{% endmermaid %}
