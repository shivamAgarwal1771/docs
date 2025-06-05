RUN npm config set proxy http://163.116.128.80:8080 && \
    npm config set https-proxy http://163.116.128.80:8080 && \
    npm config set strict-ssl false && \
    npm config set registry https://registry.npmjs.org/
