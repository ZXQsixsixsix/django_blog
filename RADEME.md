# deploy blog

```
system：CentOS Linux release 7.6.1810 (Core)

[root@xquan ~]# yum install -y nginx python3 python3-pip git 
[root@xquan ~]# pip3 install virtualenv
[root@xquan ~]# mkdir django
[root@xquan ~]# cd django
[root@xquan django]# virtualenv --python=python3.6 env
[root@xquan django]# git clone https://github.com/ZXQsixsixsix/django_blog.git
[root@xquan django]# cd django_blog
[root@xquan django]# source env/bin/activate
(env) [root@xquan django_blog_tutorial-master]# pip3 install -r requirements.txt
(env) [root@xquan django_blog_tutorial-master]# python3 manage.py collectstatic
(env) [root@xquan django_blog_tutorial-master]# python3 manage.py migrate
(env) [root@xquan django_blog_tutorial-master]# python3 manage.py runserver 0.0.0.0:8000
# Test browser access 
http://YOU_IP:8000/article/article-list/
(env) [root@xquan django_blog_tutorial-master]# pip3 install gunicorn
(env) [root@xquan django_blog_tutorial-master]# yum install -y nginx
[root@xquan nginx]# cat conf.d/zhangxiuquan.conf 
server {
	    charset utf-8;
	        listen 80;
		    server_name YOU_IP_OR_YOU_DOMAIN;
		     
		        location /static {
				        alias /root/django/django_blog_tutorial-master/collectstatic;
					    }
			location /media {
					alias /root/django/django_blog_tutorial-master/media;
					    }
			    location / {
				            proxy_set_header Host $host;
					            proxy_pass http://127.0.0.1:8000;
						        }
}
(env) [root@xquan django_blog_tutorial-master]# systemctl start nginx
(env) [root@xquan django_blog_tutorial-master]# systemctl enable nginx
(env) [root@xquan django_blog_tutorial-master]# gunicorn my_blog.wsgi -w 2 -k gthread -b 0.0.0.0:8000
# browser access
http://YOU_IP
```
