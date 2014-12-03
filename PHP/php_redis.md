
<h1>centos下 php安装redis扩展</h1>
<hr />
<ol>
<li>redis 和 php 的安装
   对于<strong>redis</strong> 和 <strong>php</strong> 的安装,这里就不多说的</li>
<li>
<p>下载php redis扩展包</p>
<p>为了后续方便从GITHUB上面拉东东，首先装下<code>git-core</code></p>
<pre><code>yum install git-core
git clone git://github.com/owlient/phpredis.git
cd  phpredis
/usr/local/php/bin/phpize
./configure -with-php-config=/usr/local/php/bin/php-config
make 
make install
</code></pre>

<p>安装完成之后,会有一个<strong>redis.so</strong>的路径,要用到!!</p>
<h5>ps: <strong>phpize</strong> 的路径需使用自己环境的路径,可以用</h5>
<pre><code>which phpize
</code></pre>

<h5>命令来获取路径!</h5>
</li>
<li>
<p>修改 <code>php.ini</code> 配置</p>
<p>在 <code>php.ini</code> 文件中添加</p>
<pre><code>extension=redis.so
</code></pre>

<p>注意<strong>redis.so</strong>的路径</p>
</li>
<li>
<p>重启 <code>php</code>, 查看 phpInfo 中是否有<code>redis</code>模块</p>
</li>
<li>
<p>用以下代码测试是否可以工作</p>
<pre><code>&lt;?php
    $redis= newRedis();
    $redis-&gt;connect('127.0.0.1',6379);
    $redis-&gt;set('name','xxx');
    echo$redis-&gt;get('name');
?&gt;
</code></pre>

<p>如果遇到<code>Fatal error: Uncaught exception 'RedisException' with message 'Redis server went away'</code>的问题,那是因为你没有打开<code>redis</code>,启动<code>redis</code> 服务就可以了!!</p>
</li>
</ol>

