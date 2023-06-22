# Machine Learning Knowledge Base

A Knowledge Base on Machine Learning and Statistics.


# Build HTML site

Build the HTML:

```
$ .\make html
```

The above works on Powershell. One UNIX systems run `make html` and on Windows command line run `make.bat html`.

To serve the page locally you can use Python built-in server.

```
$ cd _build/html 
$ python -m http.server
```

To deploy and serve automatically on changes to the source files:

```
$ sphinx-autobuild . _build/html
```

## Deploy to Github Pages

Deploy to Github Pages with `ghp-import`:

```
$ ghp-import -n -p -f _build/html
```
