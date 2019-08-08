url重写是指通过配置conf文件，以让网站的url中达到某种状态时则定向/跳转到某个规则，比如常见的伪静态、301重定向、浏览器定向等


[nginx配置url重写][https://xuexb.com/post/nginx-url-rewrite.html]
[搞懂nginx的rewrite模块][https://segmentfault.com/a/1190000008102599]


    
    rewrite ^/(\d+).html /seo/?servicetype=$1 last;
    rewrite ^/(\d+)/(\d+).html /seo/?servicetype=$1&catid=$2 last;
    rewrite ^/(\d+)/area-(\d+).html /seo/?servicetype=$1&catid=0&areaid=$2 last;
    rewrite ^/(\d+)/(\d+)/(\d+).html /seo/?servicetype=$1&catid=$2&id=$3 last;