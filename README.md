# Documentation
This is the markdown source for the Lobaro online documentation. It is generated using mkdocs.

The generated documentation is available at https://docs.lobaro.com

# Local Setup

Install Python: https://www.python.org/downloads/

Install Python dependencies (in this directory):

    pip install -r requirements.txt


Run mkDocs

    mkdocs serve

## Features
For a feature description and general markdown information consult:

- [Cinder theme features](https://sourcefoundry.org/cinder/specimen/) 
- [mkdocs wirting your docs](https://www.mkdocs.org/user-guide/writing-your-docs/) 
- [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet)
- [code display programming languages overview](https://highlightjs.org/static/demo/)
- [Diagrams syntax](https://mermaidjs.github.io/sequenceDiagram.html)



## Legacy
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