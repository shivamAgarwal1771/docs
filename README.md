curl -O https://nodejs.org/dist/v20.14.0/node-v20.14.0-linux-x64.tar.xz
tar -xf node-v20.14.0-linux-x64.tar.xz
sudo mv node-v20.14.0-linux-x64 /usr/local/node20
echo 'export PATH=/usr/local/node20/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
