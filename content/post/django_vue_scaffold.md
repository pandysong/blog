---
date: 2021-12-09
title: Creating Django and Vue Scaffold
weight: 10
---

# Introduction

This describe how we could create an application Scaffold to use Vue at the
frontend and use Django on the backend. Both Vue and Django support hot-reload
so that both could be developed in parallel.

The template could be found in this [repo](https://github.com/pandysong/django_vue_scaffold).

# Creating Django project

This is just following the Django tutorial, so if you are familar with it, just
do what you normally do.

## create an virtual env

Creating a virtual environment is not a must, however in order to isolate the
environment, it is recommended to do so. You might want to use pipenv or other
tools.

```
mkdir django_vue_scaffold
cd django_vue_scaffold
mkdir .venv
python3 -m venv .venv
```

And activate the venv:

```
source .venv/bin/activate
```

The command line looks like

```
(.venv) ~/project/django_vue_scaffold
```

## Install Django on this venv

```
python3 -m pip install django
```

## Start a Django Project

```

django-admin startproject django_vue_scaffold .
```

now the project looks like:

```
(.venv) ~/project/django_vue_scaffold ls -1
django_vue_scaffold
manage.py
```

## create Django app 'api'

```
django-admin startapp api
```

## Django migrate the database

```
python3 manage.py migrate
```

## Now run the server


```
python3 manage.py runserver
```

This should run a web server on 

```
http://127.0.0.1:8000/
```

You could check from your browser that it works

![django works in browser](/img/django_127_0_0_1.png)

## freeze the packages

So that you could install all the required packages:

```
python3 -m pip freeze > requirements.txt
```


# Creating Vue frontend

```
vue init webpack frontend
```

After answering a couple of questions, the vue command line will create the vue
project at the `frontend` directory.

Try to run the development environment in vue:

```
DONE  Compiled successfully in 5897ms

Your application is running here: http://localhost:8080
```

Open your browser, following figure should be displayed:


![vue running locally](/img/vue_127_0_0_1.png)


# Add Restful api call to Vue application

Here we use axios to make request to the backend:

```
npm install --save axios
```

Then add the axios call in frontend:

Details could be found on the repo: git@github.com:pandysong/django_vue_scaffold.git


Following code just add a form the HelloWorld component and when clicked, it
send an post request via axios:

```
diff --git a/frontend/src/components/HelloWorld.vue b/frontend/src/components/HelloWorld.vue
index 1c19f2a..4399e0a 100644
--- a/frontend/src/components/HelloWorld.vue
+++ b/frontend/src/components/HelloWorld.vue
@@ -1,7 +1,14 @@
 <template>
   <div class="hello">
-    <h1>{{ msg }}</h1>
     <h2>Essential Links</h2>
+
+      <form>
+        <input type="file" name="file" @change="handleFile">
+        <div class="submit">
+        <button type="submit" @click="submitFile">submit</button>
+        </div>
+      </form>
+
     <ul>
       <li>
         <a
@@ -84,11 +91,55 @@
 </template>
 
 <script>
+import axios from 'axios'
+
+axios.defaults.xsrfHeaderName = 'X-CSRFToken'
+axios.defaults.xsrfCookieName = 'csrftoken'
+axios.defaults.withCredentials = true
+
 export default {
   name: 'HelloWorld',
-  data () {
-    return {
-      msg: 'Welcome to Your Vue.js App'
+  data: () => ({
+    file: ''
+  }),
+  components: {
+  },
+  methods: {
+    submitFile () {
+      // send a GET request to backend and get teh csrftoken from cookie
+      // we will cover it more in the backend parts
+      fetch('http://localhost:8080/api/csrf')
+
+      // The csrftoken is embedded in the cookie
+      const csrfToken = this.getCookie('csrftoken')
+      // console.log(csrfToken)
+      let formData = new FormData()
+      formData.append('file', this.file)
+      console.log('>> formData >> ', formData)
+
+      // You should have a server side REST API
+      axios.post('http://localhost:8000/api/upload',
+        formData, {
+          headers: {
+            'Content-Type': 'multipart/form-data',
+            'X-CSRFToken': csrfToken
+          }
+        })
+        .then(function () {
+          console.log('SUCCESS!!')
+        })
+        .catch(function () {
+          console.log('FAILURE!!')
+        })
+    },
+    handleFile (event) {
+      this.file = event.target.files[0]
+      console.log('>>>> 1st element in files array >>>> ', this.file)
+    },
+    getCookie (name) {
+      var value = '; ' + document.cookie
+      var parts = value.split('; ' + name + '=')
+      if (parts.length === 2) return parts.pop().split(';').shift()
     }
   }
 }
```

# Add Restful handling at the backend

Notes for the following code

- it adds two api under 'api'
    
    - csrf: to get the csrf token via the GET request.
    - upload: actually Post the data to, CORS (Cross-origin resource sharing)
      means here we have two origin one is localhost:8080 (running frontend)
      and another one 127.0.0.1:8000, two origin is interacting with each
      other. In order to tackle security issue, we have to pass a csrf token
      when posting data (which is sensitive) to server.

```
diff --git a/api/urls.py b/api/urls.py
new file mode 100644
index 0000000..678ecf7
--- /dev/null
+++ b/api/urls.py
@@ -0,0 +1,8 @@
+from django.urls import path
+
+from . import views
+
+urlpatterns = [
+    path('upload', views.handle_upload, name='api-upload'),
+    path('csrf', views.get_csrf, name='api-csrf'),
+]
diff --git a/api/views.py b/api/views.py
index 91ea44a..f5d90b6 100644
--- a/api/views.py
+++ b/api/views.py
@@ -1,3 +1,27 @@
 from django.shortcuts import render
+from django.shortcuts import render
+from django.views.decorators.http import require_http_methods
+from django.http import HttpResponse, JsonResponse
+from django.views.decorators.csrf import ensure_csrf_cookie
+from django.middleware import csrf
 
 # Create your views here.
+
+
+@require_http_methods(["POST", "OPTIONS"])
+def handle_upload(request):
+    # I can assume now that only GET or POST requests make it this far
+    # ...
+    response = HttpResponse()
+    return response
+
+
+@ensure_csrf_cookie
+def get_csrf(request):
+    ''' return csrf cookie
+        to the client so that client could POST to server
+        using the token
+    '''
+    response = JsonResponse({'detail': 'CSRF cookie set'})
+    response['X-CSRFToken'] = csrf.get_token(request)
+    return response
diff --git a/django_vue_scaffold/settings.py b/django_vue_scaffold/settings.py
index c48e0c2..c5c686f 100644
--- a/django_vue_scaffold/settings.py
+++ b/django_vue_scaffold/settings.py
@@ -37,6 +37,7 @@ INSTALLED_APPS = [
     'django.contrib.sessions',
     'django.contrib.messages',
     'django.contrib.staticfiles',
+    'api',
 ]
 
 MIDDLEWARE = [
diff --git a/django_vue_scaffold/urls.py b/django_vue_scaffold/urls.py
index f894bc4..5ec9c2e 100644
--- a/django_vue_scaffold/urls.py
+++ b/django_vue_scaffold/urls.py
@@ -14,8 +14,10 @@ Including another URLconf
     2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
 """
 from django.contrib import admin
-from django.urls import path
+from django.urls import path, include
 
 urlpatterns = [
     path('admin/', admin.site.urls),
+    path('api/', include('api.urls')),
+
 ]
```

Besides above changes, we have to add corsheaders  to the middleware:

```
diff --git a/django_vue_scaffold/settings.py b/django_vue_scaffold/settings.py
index c5c686f..e0adb1f 100644
--- a/django_vue_scaffold/settings.py
+++ b/django_vue_scaffold/settings.py
@@ -37,12 +37,14 @@ INSTALLED_APPS = [
     'django.contrib.sessions',
     'django.contrib.messages',
     'django.contrib.staticfiles',
+    'corsheaders',
     'api',
 ]
 
 MIDDLEWARE = [
     'django.middleware.security.SecurityMiddleware',
     'django.contrib.sessions.middleware.SessionMiddleware',
+    'corsheaders.middleware.CorsMiddleware',
     'django.middleware.common.CommonMiddleware',
     'django.middleware.csrf.CsrfViewMiddleware',
     'django.contrib.auth.middleware.AuthenticationMiddleware',
@@ -50,6 +52,17 @@ MIDDLEWARE = [
     'django.middleware.clickjacking.XFrameOptionsMiddleware',
 ]
 
+CORS_ALLOW_CREDENTIALS = True
+CORS_ORIGIN_WHITELIST = (
+    'http://127.0.0.1:8080',
+    'http://localhost:8080',
+)
+
+CSRF_TRUSTED_ORIGINS = [
+    'http://127.0.0.1:8080',
+    'http://localhost:8080',
+]
+
 ROOT_URLCONF = 'django_vue_scaffold.urls'
 
 TEMPLATES = [
```

We also need to install the middleware via pip:

```
python3 -m pip install django-cors-headers
```

# Done

Now you could post an file to backend and backend will return the file size by
json message and frontend will display it just below the form.
