# Documentation
This is the markdown source for the Lobaro online documentation. It is generated using mkdocs.

The generated documentation is available at https://docs.lobaro.com

## Local Setup

Install Python: https://www.python.org/downloads/

Install Python dependencies (in this directory):

    pip install -r requirements.txt


Run mkDocs

    mkdocs serve

### Features
For a feature description and general markdown information consult:

- [Material theme features](https://squidfunk.github.io/mkdocs-material/)
- [Styling Features](https://yakworks.github.io/mkdocs-material-components/blocks/)
- [pymdown addons](https://squidfunk.github.io/mkdocs-material/extensions/pymdown/) 
- [(old) Cinder theme features](https://sourcefoundry.org/cinder/specimen/) 
- [mkdocs writing your docs](https://www.mkdocs.org/user-guide/writing-your-docs/) 
- [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet)
- [code display programming languages overview](https://highlightjs.org/static/demo/)
- [Diagrams syntax](https://mermaidjs.github.io/sequenceDiagram.html)


**Extentions**
[attr_list](https://python-markdown.github.io/extensions/attr_list/) 

**Scaled images**

    ![Alternative Text](url/to/img.png){: style="height:40px;width:143px"}`
    
**URL open in new window**

    [URL Text](http://url.example/index.html){: target="_blank"}
    
For e.g. copyright information use: [Footnotes](https://squidfunk.github.io/mkdocs-material/extensions/footnotes/)

**Info / Warning / Question / Bug / ...**
    
    !!! info "Heading"
    
    More text explaining in detail.
    
    Another paragraph.
    
Instead of `info` you can use `tip`, `success`, `question`, `warning`, `failure`, `danger`, `bug`, 
`example`, `quote`. 
See [Admonition](https://squidfunk.github.io/mkdocs-material/extensions/admonition/).


### Legacy
For legacy online documentation see [repo wiki](https://github.com/Lobaro/docs/wiki).

## Charts
This documentation uses [MermaidJS][mermaid] to render flow charts etc. Charts are directly 
defined within the markdown source code and will be rendered in the client using 
JavaScript. The integration of mermaid into mkdocs is realised using the 
`pymdown-extensions`. Charts are defined in source code listings (with triple backticks ```) 
with the custom language `mermaid` defined. This solution is taken from a comment to an 
issue on github:  
https://github.com/squidfunk/mkdocs-material/issues/693#issuecomment-411885426

[mermaid]: https://mermaidjs.github.io/
