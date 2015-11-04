The following worked for me to get my .tex files back after I messed up my sharelatex installation.

## Dump Database Contents
``mongodump

## Convert Project Store to JSON Format
``cd dump/sharelatex/
``bsondump projects.bson projects.json

## Find Project

``grep My-Title-or-Something-Else projects.json >my-project.json

Check that you only got one project:
``wc -l my-project.json

## Extract Files from Root Folder 

``mkdir my-project
``python
``import json
``f = open('my-project.json', 'rb')
``j = json.load(f)
``j.keys()
``j['name']
``rf = j['rootFolder']
``rf.keys()
``len(rf)
``docs = rf[0]
``docs.keys()
``docs2 = docs['docs']
``len(docs2)
``for item in docs2:
``    name = item['name']
``    lines = item['lines']
``    out = open('my-project/' + name, 'wb')
``    for line in lines:
``        out.write(line)
``        out.write('\n')
``    out.close()

## Pretty-Converting BSON to JSON

It's a lot easier to read the json files and to understand the structure as a human if they are pretty-printed.

### Install Go Compiler
If you don't have it yet.

``wget https://storage.googleapis.com/golang/go1.5.1.linux-amd64.tar.gz
``tar -xzf go1.5.1.linux-amd64.tar.gz
``export GOROOT=$HOME/go
``export PATH=$PATH:$GOROOT/bin

### Install Recent Bsondump
If your bsondump doesn't support --pretty

``git clone https://github.com/mongodb/mongo-tools.git
``cd mongo-tools/
``cat README.md 
``. ./set_gopath.sh
``go build -o ~/bin/bsondump bsondump/main/bsondump.go 