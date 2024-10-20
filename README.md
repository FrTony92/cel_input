# cel_input
Hits &amp; Tips about using Common Expression Langage input

Setup a test environement to use API call based on NodeRed.</br>

Start Nodered Docker container and map a volume in `/data/echange` to set some used file:</br>
```
mkdir /data/echange
docker run -it -d -p 1880:1880 -v /data/echange/:/data/echange --name mynodered nodered/node-red:latest
```

Import the following code to create 2 APIs calls in NodeRed:</br>
[NodeRed_flow.json](/NodeRed_flow.json)</br>

The firts one will answer a API Token.</br>
The second one will take a jason file in the NodeRed volume and send it as the response of the API call.</br>
You can export API result in a JSON file and reuse it for test.</br>