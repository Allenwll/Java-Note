# ssh免密码登录

在master与slave3之间配置免密码登录

## 主机信息

### master:

    [hadoop<span class="hljs-annotation">@master</span> ~]$ cat /etc/sysconfig/network
    <span class="hljs-type">HOSTNAME</span>=master
    <span class="hljs-type">NETWORKING</span>=yes

    [hadoop<span class="hljs-annotation">@master</span> ~]$ cat /etc/hosts
    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">127.0</span></span>.<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">0.1</span></span>       localhost<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain</span>   localhost<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain</span>   localhost4      localhost4<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain4</span> localhost
    ::<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">1</span></span>     localhost<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain</span>   localhost<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain</span>   localhost6      localhost6<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain6</span> localhost

    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">192.168</span></span>.<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">163.177</span></span> master<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.hadoop</span> master
    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">192.168</span></span>.<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">163.178</span></span> slave3<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.hadoop</span> slave3
    `</pre>

    ### slave3:

    <pre>`[hadoop<span class="hljs-annotation">@slave</span>3 .ssh]$ cat /etc/sysconfig/network
    <span class="hljs-type">NETWORKING</span>=yes
    <span class="hljs-type">HOSTNAME</span>=slave3
    <span class="hljs-type">NTPSERVERARGS</span>=iburst

    [hadoop<span class="hljs-annotation">@slave</span>3 .ssh]$ cat /etc/hosts
    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">127.0</span></span>.<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">0.1</span></span>   localhost localhost<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain</span> localhost4 localhost4<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain4</span>
    ::<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">1</span></span>         localhost localhost<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain</span> localhost6 localhost6<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.localdomain6</span>

    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">192.168</span></span>.<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">163.177</span></span> master<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.hadoop</span> master
    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">192.168</span></span>.<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">163.178</span></span> slave3<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.hadoop</span> slave3
    `</pre>

    ## 配置

    ### <span class="hljs-number">1.</span> slave3

    <pre>`ssh-keygen -t rsa 三次回车
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    ssh-<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-keyword"</span>>copy</span>-<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-property"</span>>id</span> -i .ssh/id_rsa.pub hadoop<span class="hljs-annotation">@master</span>

    [hadoop<span class="hljs-annotation">@slave</span>3 .ssh]$ ls -al
    总用量 <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">24</span></span>
    drwx<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>------. <span class="hljs-number">2</span> hadoop hadoop <span class="hljs-number">4096</span> <span class="hljs-number">12</span>月  <span class="hljs-number">4</span> <span class="hljs-number">16</span>:<span class="hljs-number">55</span> .</span>
    drwx<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>------. <span class="hljs-number">5</span> hadoop hadoop <span class="hljs-number">4096</span> <span class="hljs-number">12</span>月  <span class="hljs-number">4</span> <span class="hljs-number">16</span>:<span class="hljs-number">55</span> ..</span>
    -rw<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>-------. <span class="hljs-number">1</span> hadoop hadoop  <span class="hljs-number">790</span> <span class="hljs-number">12</span>月  <span class="hljs-number">4</span> <span class="hljs-number">17</span>:<span class="hljs-number">22</span> authorized_keys</span>
    -rw<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>-------. <span class="hljs-number">1</span> hadoop hadoop <span class="hljs-number">1675</span> <span class="hljs-number">12</span>月  <span class="hljs-number">4</span> <span class="hljs-number">16</span>:<span class="hljs-number">55</span> id_rsa</span>
    -rw-r<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>--r--. <span class="hljs-number">1</span> hadoop hadoop  <span class="hljs-number">395</span> <span class="hljs-number">12</span>月  <span class="hljs-number">4</span> <span class="hljs-number">16</span>:<span class="hljs-number">55</span> id_rsa.pub</span>
    -rw-r<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>--r--. <span class="hljs-number">1</span> hadoop hadoop  <span class="hljs-number">404</span> <span class="hljs-number">12</span>月  <span class="hljs-number">4</span> <span class="hljs-number">16</span>:<span class="hljs-number">55</span> known_hosts</span>
    `</pre>

    ### <span class="hljs-number">2.</span> master

    <pre>`ssh-keygen -t rsa 三次回车
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    ssh-<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-keyword"</span>>copy</span>-<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-property"</span>>id</span> -i .ssh/id_rsa.pub hadoop<span class="hljs-annotation">@slave</span>3

    [hadoop<span class="hljs-annotation">@master</span> .ssh]$ ls -al
    total <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-number"</span>><span class="hljs-number">24</span></span>
    drwx<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>------. <span class="hljs-number">2</span> hadoop hadoop <span class="hljs-number">4096</span> <span class="hljs-type">Sep</span> <span class="hljs-number">26</span> <span class="hljs-number">16</span>:<span class="hljs-number">02</span> .</span>
    drwx<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>------. <span class="hljs-number">7</span> hadoop hadoop <span class="hljs-number">4096</span> <span class="hljs-type">Dec</span>  <span class="hljs-number">4</span> <span class="hljs-number">16</span>:<span class="hljs-number">50</span> ..</span>
    -rw<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>-------. <span class="hljs-number">1</span> hadoop hadoop <span class="hljs-number">1580</span> <span class="hljs-type">Dec</span>  <span class="hljs-number">4</span> <span class="hljs-number">16</span>:<span class="hljs-number">56</span> authorized_keys</span>
    -rw<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>-------. <span class="hljs-number">1</span> hadoop hadoop <span class="hljs-number">1675</span> <span class="hljs-type">Sep</span> <span class="hljs-number">26</span> <span class="hljs-number">15</span>:<span class="hljs-number">59</span> id_rsa</span>
    -rw-r<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>--r--. <span class="hljs-number">1</span> hadoop hadoop  <span class="hljs-number">395</span> <span class="hljs-type">Sep</span> <span class="hljs-number">26</span> <span class="hljs-number">15</span>:<span class="hljs-number">59</span> id_rsa.pub</span>
    -rw-r<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-comment"</span>>--r--. <span class="hljs-number">1</span> hadoop hadoop <span class="hljs-number">2396</span> <span class="hljs-type">Dec</span>  <span class="hljs-number">4</span> <span class="hljs-number">17</span>:<span class="hljs-number">22</span> known_hosts</span>
    `</pre>

    ## 使用

    从master免密码登录slave3

    <pre>`<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-attr_selector"</span>>[hadoop<span class="hljs-annotation">@master</span> ~]</span>$ <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>ssh</span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>slave3</span>
    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>><span class="hljs-type">Last</span></span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>login</span>: <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>><span class="hljs-type">Mon</span></span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>><span class="hljs-type">Dec</span></span>  <span class="hljs-number">4</span> <span class="hljs-number">17</span><span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-pseudo"</span>>:<span class="hljs-number">26</span></span><span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-pseudo"</span>>:<span class="hljs-number">11</span></span> <span class="hljs-number">2017</span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>from</span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>master</span><span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.hadoop</span>
    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-attr_selector"</span>>[hadoop<span class="hljs-annotation">@slave</span>3 ~]</span>$
    `</pre>

    从slave3免密码登录master

    <pre>`<span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-attr_selector"</span>>[hadoop<span class="hljs-annotation">@slave</span>3 ~]</span>$ <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>ssh</span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>master</span>
    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>><span class="hljs-type">Last</span></span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>login</span>: <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>><span class="hljs-type">Mon</span></span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>><span class="hljs-type">Dec</span></span>  <span class="hljs-number">4</span> <span class="hljs-number">17</span><span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-pseudo"</span>>:<span class="hljs-number">22</span></span><span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-pseudo"</span>>:<span class="hljs-number">44</span></span> <span class="hljs-number">2017</span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>from</span> <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-tag"</span>>slave3</span><span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-class"</span>>.hadoop</span>
    <span <span class="hljs-class"><span class="hljs-keyword">class</span>=</span><span class="hljs-string">"hljs-attr_selector"</span>>[hadoop<span class="hljs-annotation">@master</span> ~]</span>$

### 关键点

_注意点authorized_keys权限为600 .ssh权限为700_