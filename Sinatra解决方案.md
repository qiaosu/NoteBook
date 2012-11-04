Sinatra
=======

Sinatra特点：

* 自己选择ORM 目前比较习惯datamapper
* 自己选择template engine
* 路由设定可简单可复杂

####路由

路由按照它们被定义的顺序进行匹配，第一个与请求匹配的路由会被调用。

**before和after**

<pre><code>before '/users'
get '/users/:id'
after '/users/'</code></pre>

可以有多个before和after，先载入的先执行。

**Splat Param Route**

路由中的*号会被集中到特定的参数变量params[:splat]中。

<pre><code>get '/say/*/to/*' do
  # /say/hello/to/world
  params[:splat] # => ["hello", "world"]
end</code></pre>

**路由可以接受正则表达式**

<pre><code>get %r{/posts/name-([\w]+)} do
  # => get /posts/name-abc, params[:captures][0] = 'abc'
  "Hello, #{params[:captures].first}!"  
end</code></pre>

**RESTful**

basic: get/post/put/delete<br>
patch, options and more...

**Route Return Values**

HTTP Status Code<br>
Headers<br>
Response Body<br>

####session

<pre><code>enable :sessions # equal to 「use Rack::Session::Cookie」

post '/sessions' do 
  @current_user = User.auth(params[:account], params[:passwd]) 
  session[:uid] = @current_user.id if @current_user
end 
delete '/sessions' do
  session[:uid] = nil
  redirect '/' 
end</code></pre>

####cookie

<pre><code>get '/' do 
  response.set_cookie('foo', :value => 'BAR') 
  response.set_cookie("thing", { :value => "thing2",
                                 :domain => 'localhost', 
                                 :path => '/', 
                                 :expires => Time.today, 
                                 :secure => true, 
                                 :httponly => true })
end
get '/readcookies' do 
  cookies['thing'] # => thing2
end</code></pre>

####Helpers

filters, routes, templates中都可以使用helpers中定义的帮助方法。

####ORM-datamapper

<http://datamapper.org/docs/>

<br>


###sinatra-more

`widget`

<https://github.com/nesquena/sinatra_more>

<br>

###sinatra-authentication

`widget` 

<https://github.com/codebiff/sinatra-authentication>

<br>

###sinatra-head-cleaner

`widget` 

<https://github.com/daz4126/sinatra-head-cleaner>

<br>

###sinatra-synchrony

`widget` 

<https://github.com/kyledrake/sinatra-synchrony>

<br>

###sinatra-assetpack

`widget` 

<https://github.com/rstacruz/sinatra-assetpack>

<br>

###Sinatra-backbone

`widget` 

<https://github.com/rstacruz/sinatra-backbone>

<br>

for more

###sinatra-support

`Utilities collection`

<https://github.com/sinefunc/sinatra-support>