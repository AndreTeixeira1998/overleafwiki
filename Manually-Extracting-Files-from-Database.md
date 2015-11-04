The following worked for me to get my .tex files back after I messed up my sharelatex installation.

Comments and improvements welcome.

## Dump Database Contents
``mongodump``
(no parameters needed; creates folder "dump")

## Convert Project Store to JSON Format
    cd dump/sharelatex/
    bsondump projects.bson projects.json

## Find Project

``grep My-Title-or-Something-Else projects.json >my-project.json``

Check that you only got one project:
``wc -l my-project.json``

TODO: What is docs.bson for? It doesn't seem to contain any user .tex files.

## Extract Files from Root Folder 

    mkdir my-project
    python

    import json
    f = open('my-project.json', 'rb')
    j = json.load(f)
    rf = j['rootFolder']
    docs = rf[0]
    docs2 = docs['docs']
    for item in docs2:
        name = item['name']
        lines = item['lines']
        out = open('my-project/' + name, 'wb')
        for line in lines:
            out.write(line)
            out.write('\n')
        out.close()

TODO: Support sub-folders.

This is it. The following gives pointers for further analysis, e.g. to find files in sub-folders.

## Pretty-Converting BSON to JSON

It's a lot easier to read the json files and to understand the structure as a human if they are pretty-printed.

### Install Recent Bsondump
If your bsondump doesn't support the "--pretty" option.

#### Install Go Compiler
If you don't have it yet.

    wget https://storage.googleapis.com/golang/go1.5.1.linux-amd64.tar.gz
    tar -xzf go1.5.1.linux-amd64.tar.gz
    export GOROOT=$HOME/go
    export PATH=$PATH:$GOROOT/bin

#### Compile Bsondump

    git clone https://github.com/mongodb/mongo-tools.git
    cd mongo-tools/
    cat README.md 
    . ./set_gopath.sh
    go build -o ~/bin/bsondump bsondump/main/bsondump.go 

### Example Usage: Get E-mail List

    bsondump --pretty users.bson users.json
    grep \"email\" users.json | cut -d\" -f4 | sort

TODO: Does this include past users? Can users have multiple e-mail addresses? Create separate wiki page for this topic?