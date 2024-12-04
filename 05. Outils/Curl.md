
```
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/ -i
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/
curl -H 'Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
curl -X POST -d '{"search":"london"}' -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php
["London (UK)"]
```