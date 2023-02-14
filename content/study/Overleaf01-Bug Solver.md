---
title: "Overleaf 01-Bug Solver"
date: 2023-02-14
draft: false
description: "Problems encountered when using Overleaf"
type: "post"
showTableOfContents: true
tags: ["LaTeX"]

---

## Reference

### How to use `.bib` file

1. Create a `xxx.bib` file in your overleaf project. Place it directly in your root content.

2. Open your main `.tex` file. Add the following two codes before `\end{document}`

   ```latex
   ...
   
   \bibliographystyle{<style>} % eg. ieeetr
   \bibliography{<bib_name>} % eg. reference
   
   \end{document}
   ```

   - `<style>`: refer to https://ja.overleaf.com/learn/latex/Bibtex_bibliography_styles
   - `<bib_name>`: your `.bib` file's name

3. Add some reference in your `.bib` file.

4. Cite one of the article from your `.bib` file. If your finish previous 3 steps without citing one article, Overleaf will <u>report an error</u>. You have to finish <u>citing at least one article</u> (`\cite{xxx}`) to prevent such error.