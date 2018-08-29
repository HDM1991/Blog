使用 cURL 函数的基本思想是先使用 curl_init() 初始化 cURL会话，接着可以通过 curl_setopt() 设置需要的全部选项，然后使用 curl_exec() 来执行会话，当执行完会话后使用 curl_close() 关闭会话。

# curl_setopt
* CURLOPT_HEADER
    默认时 false，为 true 时会将 http 头的部分也输出。可以参考如下列子

        header('Content-Type:text/plain');  

        $url =  'https://www.baidu.com/';
        $ch = curl_init($url);
        // curl_setopt($ch, CURLOPT_HEADER, true);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        $r = curl_exec($ch);
        if (!$r) {
            echo 'Curl error: ' . curl_error($ch);
            curl_close($ch);
        }
        else
        {
          echo $r;
        }
        return;


* CURLOPT_HTTPHEADER
    设置 HTTP 头字段的数组。格式： array('Content-type: text/plain', 'Content-length: 100') 

* CURLOPT_POSTFIELDS
    传递一个数组到 CURLOPT_POSTFIELDS，cURL会把数据编码成 multipart/form-data, Content-Type头将会被设置成 multipart/form-data; 传递一个 URL-encoded 字符串时，数据会被编码成 application/x-www-form-urlencoded, Content-Type头将会被设置成 application/x-www-form-urlencoded。

* CURLOPT_RETURNTRANSFER
    默认 FALSE. TRUE 将 curl_exec()获取的信息以字符串返回，而不是直接输出。   
* CURLOPT_FILE
    将 curl_exec() 获取的信息写到文件中。

# 发送 post 请求
发送给 post 请求就是要设置 CURLOPT_POST 和 CURLOPT_POSTFIELDS，如下所示

    curl_setopt($ch, CURLOPT_POST, 1);
    // curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
    curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($post_data));

设置 CURLOPT_POSTFIELDS 时，要注意是传递一个数组，还是 URL-encoded 字符串，这需要看情况而定。

# 100-continue
curl 在 post 的数据大于 1023 byte 时，会发送 100-continue 这个请求，如 [《curl耗时长问题-Expect: 100-continue》][1] 中所述。

100-continue 的使用场景就是客户端在 post 较大的数据时询问服务器是否接受，如果服务器接受，就返回一个 status 为 100 的响应。客户端在接收到这个相应时，在 post 数据。详细情况可参考[《http之100-continue》][2]

# 如何下载大文件
目前遇到的情况，采用设置 CURLOPT_FILE 的方式就够了。

    curl_setopt($ch, CURLOPT_FILE, $fp);

# 设置超时时间    
CURLOPT_TIMEOUT 允许 cURL 函数执行的最长秒数。  

CURLOPT_TIMEOUT_MS  设置cURL允许执行的最长毫秒数。 如果 libcurl 编译时使用系统标准的名称解析器（ standard system name resolver），那部分的连接仍旧使用以秒计的超时解决方案，最小超时时间还是一秒钟。  

[1]: http://www.snooda.com/read/322 "curl耗时长问题-Expect: 100-continue "
[2]: http://www.cnblogs.com/tekkaman/archive/2013/04/03/2997781.html "http之100-continue"