shivam220802@EC03-B17-UBAPP2:~/superset2/bu-digital-insightshub-backend$ sudo docker run -it --rm debian:bookworm bash
root@86f566ddf50d:/# apt-get updtae
E: Invalid operation updtae
root@86f566ddf50d:/# apt-get update
Ign:1 http://deb.debian.org/debian bookworm InRelease
Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
Ign:1 http://deb.debian.org/debian bookworm InRelease
Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
Ign:1 http://deb.debian.org/debian bookworm InRelease
Ign:2 http://deb.debian.org/debian bookworm-updates InRelease
Ign:3 http://deb.debian.org/debian-security bookworm-security InRelease
Err:1 http://deb.debian.org/debian bookworm InRelease
  Connection failed [IP: 151.101.54.132 80]
Err:2 http://deb.debian.org/debian bookworm-updates InRelease
  Connection failed [IP: 151.101.54.132 80]
Err:3 http://deb.debian.org/debian-security bookworm-security InRelease
  Connection failed [IP: 151.101.54.132 80]
Reading package lists... Done
W: Failed to fetch http://deb.debian.org/debian/dists/bookworm/InRelease  Connection failed [IP: 151.101.54.132 80]
W: Failed to fetch http://deb.debian.org/debian/dists/bookworm-updates/InRelease  Connection failed [IP: 151.101.54.132 80]
W: Failed to fetch http://deb.debian.org/debian-security/dists/bookworm-security/InRelease  Connection failed [IP: 151.101.54.132 80]
W: Some index files failed to download. They have been ignored, or old ones used instead.
root@86f566ddf50d:/# apt-get install -y curl
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package curl
