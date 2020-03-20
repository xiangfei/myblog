
# README

* npm install
* hexo server
* hexo new  ruby_traing

* hexo deploy

* hexo clean

* hexo g


## 新增集成插件

- 画图插件 mermaid

```ruby
{% mermaid %}
graph TD
A[Client] --> B[Load Balancer]
B --> C[Server01]
B --> D[Server02]

{% endmermaid %}

```

## 新增UML插件

- plantuml

```ruby
{% plantuml %}
    Bob->Alice : hello
{% endplantuml %}

```

