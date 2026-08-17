# Sharing your code

Sharing your code is essential for making your research open and reproducible, it allows other researchers to build on your work, to understand it and to verify your findings.

## How to share your code

### If you can, use open source tools

There are several reasons why people choose to use a secific software or a programming language: simplicity, familiarity with the tool, lab standards and many others. These are all valid reasons for choosing a tool, but if you want to make sure that your research is open and transparent, you should also consider **open source** tools.

Open source software grants you access to the source code and freedom to use it, modify it and distribute it according to its license.

As a consequence of using open source tools, your analysis code and experiment code if publicly available can be re-used and tested by other researchers. On the contrary, proprietary programming languages and software require the user to buy a license, or to use a license from their institution, thus contravening to the fundamental openess principle of science.

### Document your code

Code documentation is an essential prerequisite to make your research transparent and replicable. Without a decent documentation other researchers might be unable to use your code.

Documenting your code is not only advantageous for other potential users, but also for you. It is very common not to remember why you made a specific decision in you code or how some piace of code works; by providing good documentation you will save time and effort to the future you and other users. 

**Comment your code:** The best way to start documenting your code is to comment each essential piece of it while your are still programming:

- Provide explanations about inputs, outputs and types for functions and classes according to the language style.
- Provide one or more toy examples that the user can easily run.
- Provide ad hoc explanations for lines of code that might have some criticalities, or some peculiarities that might be hard to understand.

**README file:** A README file is one of the most common way to document code. It should include:

- A brief introduction explaining the scope of the code.
- A clear installation procedure.
- A list of dependencies and requirements: programming language(s), libraries or other code and files needed to make you code work. It is not enough to state the name of a library, or a programming language, but it is essential to provide also their version (e.g. matplotlib >= 3.8.0).
- Environment(s) where the code was tested/used (e.g. Ubuntu 26.04 lts).
- A clear description of the code structure and use, with a working example or more toy examples, in case you are sharing a library with different functionalities.
- References: If your code is based on articles or specific methods, provide a reference to the methods.
- Acknowledgements, list of everybody that have contributed to the project.

!!! note "Share your code according to your language standards"
    Each programming language uses specific tools and processes to ease and standardize code sharing, we recommend to follow language specific approaches to facilitate the sharing process and to allow other users to use your code.

### Choose a code hosting platform 

[Github](https://github.com/) and [Gitlab](https://gitlab.com/gitlab-org/gitlab) are just a few of the platforms that allow code sharing, such platforms, however, are not just public storages for code, but they are tools for collaborative projects. The advantage of using such platforms during your research are diverse:

- They allow to share and document your code with your publication.
- They rely on `git`, meaning that you can modify and keep track of your code locally through git and transfer (`push`) your changes to Github or other platforms.
- They allow to easily collaborate at the same project.
- They give you a free and safe backup for your code.
- They allow other people to use your code, help you to find issues and improve your code.

### Provide a license

Coming soon...

