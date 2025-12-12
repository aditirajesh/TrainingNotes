1.**FrontEnd Layer**:
   a) *Logging:* access_log logs the activities of all the visitors coming to the site. Can find which files are accessed, how NGINX responds to req and so on. error_log records events of glitches or errors. 
   
   b) *Enabling logging:* in the nginx conf file, add:
   access_log log_file log_format;
   error_log log_file log_format;
   
   c) *Write Dockerfile:* to establish the nginx:alpine image and to copy the website files into the nginx service /usr/share/nginx/html
   
   d) *Set up web service* in docker-compose.yml, copying the nginx.conf file in volumes, as well as the logs for persistence. Map port 8080:80 
   
2.**Backend Layer**:
	a) 

