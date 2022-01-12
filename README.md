# Cycads: decentralized web content

Many web contents are hosted in centralized manners for convenience.
They become unaccessible once the hosting platform is offline.
This system plan to provide web contents in Fossil SCM, which is SQLite database underneath.
Once users checkout the content locally, they are immune from any disruption of internet.
They can also update the content via checkout from latest version when online.
Because Fossil SCM is in a SQLite database, which is a single file, content can be easily transfered via e-mail, instant messenger, or any other decentralized method.

### Quick Start Guide

1. Install [Fossil SCM](https://fossil-scm.org)
2. Clone this git repository to create the directory structure
3. Compile `althttpd` via `bin/compile` or modify it to fit your system
4. Clone web content into`repositories/` with assigned working directory under `default.website`. See **Example** for details.
5. Start http server ``bin/althttpd -root `pwd` -port 8080`` or `bin/serve` for your convenience.
6. Open `localhost:8080/index.html` in web browser to see all checked out web content

### Example

You can check out Ruby API at http://.../ruby-3.0-doc

```
    fossil clone --workdir default.website/ruby-3.0-doc http://.../ruby-3.0-doc repositories/ruby-3.0-doc
```

It would put a Fossil SCM under `repositories` directory and content in `default.website/ruby-3.0-doc`

The final directory structure looks like this:

```
.
├── repositories
│   └── ruby-3.0-doc.fossil
├── default.website
│   ├── index.html
│   └── ruby-3.0-doc
│       └── index.html
├── bin
│   └── althttpd
└── src
```

You brower will find the `http://localhost:8080/ruby-3.0-doc/index.html` at `./default.website/ruby-3.0-doc/index.html`

### Fetch web content

Under directory `repositories`, do

```
    fossil clone --no-open http://.../content.fossil 
```

This only download web content in Fossil SCM format.

### Populate web content 

Under Cycads directory, do

```
    fossil open ./repositories/content.fossil --workdir default.website/content
```

### Fetch and populate web content

Under Cycads directory, do

```
    fossil clone --workdir default.website/content http://.../content repositories/content
```


### Update web content

You can update web content by run `fossil update` in the working directory (each directory under `default.website`).
You can also run `bin/update_all` to automatically update all directories.

### FAQ

**Why Fossil instead of Git**

My instinct tells me that, but I have no proof that Fossil is better.

**Why host Cycads on Github, not Fossil**

Simply because Github has more users. People are more familiar with it.

**Why index.html does not show up in browser**

Althttpd treats any file with executable permission as CGI. If index.html is marked as executable, Althttpd will try to run it instead of serving the file. Change its permission to 644 (`chmod 644 index.html`) may help.
