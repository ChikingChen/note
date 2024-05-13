# django cookie

## cookie与session

session完成会话跟踪，依赖cookie技术

## django cookie语法

- 设置cookie

  ```python
  rep.set_cookie(key,value,...) 
  rep.set_signed_cookie(key,value,salt='加密盐',...)
  ```

  设置服务器的cookie

- 获取cookie

  ```python
  request.COOKIES.get(key)
  ```

  获取请求中的cookie

- 删除cookie

  ```python
  rep =HttpResponse || render || redirect 
  rep.delete_cookie(key)
  ```

  删除请求中的cookie

## django cookie示例

- models.py

  ```python
  class UserInfo(models.Model):
      username = models.CharField(max_length=32)
      password = models.CharField(max_length=64)
  ```

- urls.py

  ```python
  from django.contrib import admin
  from django.urls import path
  from cookie import views
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('login/', views.login),
      path('index/', views.index),
      path('logout/', views.logout),
      path('order/', views.order)
  ]
  ```

- **views.py（主要关注）**

  ```python
  def login(request):
      if request.method == "GET":
          return render(request, "login.html")
      username = request.POST.get("username")
      password = request.POST.get("pwd")
  
      user_obj = models.UserInfo.objects.filter(username=username, password=password).first()
      print(user_obj.username)
  
      if not user_obj:
          return redirect("/login/")
      else:
          rep = redirect("/index/")
          rep.set_cookie("is_login", True)
          # 在返回数据中设置cookie
          # HttpResponse 即为 django 中的 http 格式数据
          return rep
          
  def index(request):
      print(request.COOKIES.get('is_login'))
      status = request.COOKIES.get('is_login') # 收到浏览器的再次请求,判断浏览器携带的cookie是不是登录成功的时候响应的 cookie
      if not status:
          return redirect('/login/')
      return render(request, "index.html")
  
  
  def logout(request):
      rep = redirect('/login/')
      rep.delete_cookie("is_login")
      return rep # 点击注销后执行,删除cookie,不再保存用户状态，并弹到登录页面
      
  def order(request):
      print(request.COOKIES.get('is_login'))
      status = request.COOKIES.get('is_login')
      if not status:
          return redirect('/login/')
      return render(request, "order.html")
  ```

  

