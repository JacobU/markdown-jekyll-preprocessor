# Markdown-Jekyll Preprocessor 

Run with bash script (one argument: file to be processed):

```
processMarkdown ()
{
    python getHeaders.py $@ 
    markdown_py temp.txt > temp.html 
    python changeToOl.py temp.html 
    python placeHeaders.py $@ olHeaders.txt 
    python writeFootnote.py temp1.txt temp2.txt 
    python getSmall.py temp1.txt temp3.txt 
    cat temp2.txt temp3.txt > processedDocument.md 
    rm temp.txt temp.html olHeaders.txt temp1.txt temp2.txt temp3.txt;
}
```
Includes a Markdown -> HTML script, which you can find at: https://pypi.org/project/Markdown/ 

## Limitations
- Can't include footnotes in headers (this would make the parsing much harder)

## Input

Takes a text file formatted with Markdown and Jekyll frontmatter. Eg:

```
---
Jekyll frontmatter
---

Markdown body
```
All footnotes are denoted by the ```<small> Example note </small>``` tag.

## Output

Produces a copy of the original document with an added table of contents, footnotes, and sidenotes. Eg:

### Input:

```
---
Jekyll frontmatter
---
# Heading 1
Text.<small>Footnote 1</small>
## Heading 2
More text.
Even more text.<small>Footnote 2</small>. Further text.<small>Footnote 3</small>
## Heading 3
# Heading 4
```
### Output:
```
---
Jekyll frontmatter
---

<div class='toc>
<ol>
    <li>Heading 1</li>
        <ol>
            <li>Heading 2</li>
            <li>Heading 3</li>
        </ol>
    <li>Heading 4</li>
</ol>

# Heading 1
Text.<small>Footnote 1</small>[^1]
## Heading 2
More text.
Even more text.<small>Footnote 2</small>[^2] Further text.<small>Footnote 3</small>[^3]
## Heading 3
# Heading 4

[^1]: Footnote 1
[^2]: Footnote 2
[^3]: Footnote 3
```
## Separate transformations
### Table of Contents
All headers are taken and included in a HTML table of contents below the frontmatter. Eg:

```
---
Jekyll frontmatter
---
# Heading 1
## Heading 2
## Heading 3
# Heading 4
```
Produces: 
```
---
Jekyll frontmatter
---

<div class='toc>
<ol>
    <li>Heading 1</li>
        <ol>
            <li>Heading 2</li>
            <li>Heading 3</li>
        </ol>
    <li>Heading 4</li>
</ol>

# Heading 1
## Heading 2
## Heading 3
# Heading 4
```

### Footnotes
All instances of ```<small></small>``` are taken and formatted in Markdown-footnote style. Eg:

```
Text.<small>Footnote 1</small>
More text.
Even more text.<small>Footnote 2</small>. Further text.<small>Footnote 3</small>
```
Produces:
```
Text.<small>Footnote 1</small>[^1]
More text.
Even more text.<small>Footnote 2</small>.[^2] Further text.<small>Footnote 3</small>[^3]

[^1]: Footnote 1
[^2]: Footnote 2
[^3]: Footnote 3
```

### Sidenotes
Since the ```<small></small>``` tags are left inside the document, they can be formatted with CSS to act as sidenotes. My sidenotes are styled based off Tufte CSS (https://edwardtufte.github.io/tufte-css/) where they appear on the right side of the text. 
