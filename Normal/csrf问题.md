### Django post请求出现403 forbidden 

在django模版template里请求接口时出现403禁止访问错误 

- 原因：django框架自带的csrf保护，当用post提交数据的时候，django会去检查是否有一个csrf的随机字符串，如果没有就会报错。

- 解决方案：

  - form表单请求

    ```html
    <form action="" method="post">{% csrf_token %}
    ```

  - Ajax

    ```javascript
    function csrfSafeMethod(method) {
        // these HTTP methods do not require CSRF protection
        return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
    }
    $.ajaxSetup({
        beforeSend: function(xhr, settings) {
            if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
                xhr.setRequestHeader("X-CSRFToken", csrftoken);
            }
        }
    });
    ```

    ```javascript
    在header里加上csrf
    **注**  {% csrf_token %} 无法在外部引用中获取
    ```