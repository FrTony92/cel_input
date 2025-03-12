# cel_input

[1. Purpose](#1-purpose)</br>
[2. Setup nodered container for api_tester](#2-setup-nodered-container-for-api_tester)</br>
&nbsp;&nbsp;[2.1 Create api_tester container](#21-create-api_tester-container)</br>
&nbsp;&nbsp;[2.2 Start api_tester container](#22-start-api_tester-container)</br>
[3. Setup mito environement](#3-setup-mito-environement)</br>

## 1. Purpose

Hits &amp; Tips about using Common Expression Langage input

## 2. Setup nodered container for api_tester

Setup a test environement to use API call based on NodeRed on directory `/data/api_tester`.</br>

### 2.1 Create api_tester container

Based on a dockerfile we are buiding a specific images named `nr_api_tester`.</br>

```
mkdir -p /data/api_tester
cd /data/api_tester
vi dockerfile
```

Copy / paste the following:</br>
```
# Utiliser l'image officielle Node-RED
FROM nodered/node-red:latest

# Définir le proxy en tant que variable d'environnement
ENV HTTP_PROXY=http://193.56.47.8:8080/
ENV HTTPS_PROXY=http://193.56.47.8:8080/

# Installer le module node-red-contrib-moment
RUN npm install node-red-contrib-moment node-red-contrib-calc node-red-contrib-filesystem node-red-contrib-os node-red-contrib-spreadsheet-in node-red-contrib-counter nrc-elasticsearch-nodes node-red-contrib-random-string --unsafe-perm

# Exposer le port par défaut de Node-RED
EXPOSE 1880

# Commande par défaut pour démarrer Node-RED
CMD ["npm", "start", "--", "--userDir", "/data"]
```

Build image:</br>
```
docker build -t nr_api_tester:latest .
```

### 2.2 Start api_tester container

Start `api_tester` Nodered Docker container and map a volume in `/data/echange` to set some used file:</br>
```
mkdir -p /data/api_tester/echange
docker run -it -d -p 1880:1880 -v /data/api_tester/echange/:/data/echange --name api_tester nr_api_tester:latest
```

Import the following code to create APIs calls entries in NodeRed:</br>
[NodeRed_flow.json](/NodeRed_flow.json)</br>

## 3. Setup mito environement

[MITO](https://github.com/elastic/mito/tree/dev?tab=readme-ov-file)

Ubuntu:
```
sudo apt remove golang-go
sudo rm -rf /usr/local/go
wget https://golang.org/dl/go1.24.0.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.24.0.linux-amd64.tar.gz
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
. ~/.bashrc
```
Becomes root:
```
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
. ~/.bashrc
cd /usr/local/go/src/
git clone https://github.com/elastic/mito.git
cd mito
go mod tidy
cd cmd/mito
go build
cp mito /usr/local/bin/
```
To test it use the following command from the mito directory:</br>
```
cd /usr/local/go/src/mito
mito -data example.json example.cel
```

You should have the same result that the one given by the GitHub README.md file.</br>

[[TOP]](#cel_input)
