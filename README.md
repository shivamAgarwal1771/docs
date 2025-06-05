ENV https_proxy="http://163.116.128.80:8080"
ENV http_proxy="http://163.116.128.80:8080"

# Other setup...

RUN npm config set proxy http://163.116.128.80:8080 && npm config set https-proxy http://163.116.128.80:8080
