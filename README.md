# pythonvoc

A RDF-based vocabulary to model Python into RDF. Python logic can thus be represented, queried, generated, validated, analysed, transformed and reused as semantic objects themselves.

# Abstract

The Python Vocabulary provides a formal representation of the Python programming language, enabling the modeling and generation of Python code through an abstract syntax tree (AST) framework. It defines classes and properties to describe the various components of Python syntax, including functions, classes, statements, expressions, and data structures. Additionally, it incorporates SHACL shapes for validating Python code structures and supports algorithms for serializing Python code from RDF representations. This vocabulary enhances code generation and manipulation, bridging Python with broader semantic web technologies.

# Description

The Python Vocabulary formalizes the Python programming language, offering a structured representation of its syntax and semantics. It defines classes for different Python constructs, such as 'python:StatementFunctionDef' for function definitions, 'python:StatementClassDef' for class definitions, and properties to capture relationships between these components. Central to this vocabulary is the class 'python:Module', which serves as a building block for all types of Python code segments. Each code unit can have attributes, such as 'python:decorator' for decorators on functions or classes and 'python:argument' for defining function parameters. The vocabulary also includes SHACL shapes to validate the correctness of Python code structures, ensuring that generated code adheres to Python syntax rules. These shapes facilitate the creation of well-formed Python code that can be executed reliably. This comprehensive approach allows users to leverage RDF representations to generate Python code, enabling seamless integration of semantic technologies with programming tasks.

# Introduction

In today’s fast-paced development environment, the need for effective programming practices and tools is essential for managing code complexity. As organizations strive to maintain high-quality codebases, they face challenges related to code generation, validation, and reuse. The Python Vocabulary addresses these challenges by providing a structured framework to model and generate Python code through an abstract syntax tree representation, enhancing the agility and reliability of programming practices.

# Background

The evolving landscape of software development demands tools that facilitate rapid code generation and validation, but safeguard the meaning and intent of the code. Python, with its rich ecosystem and diverse applications, requires robust frameworks to handle its syntax and semantics effectively. The complexity of Python constructs poses challenges for developers, particularly in ensuring compliance with best practices and coding standards, but also in explaining the logic of each program. The Python Vocabulary meets these challenges by offering a formalized structure for representing Python code, making it accessible now and in the future.

# Objective

To tackle the challenges of Python code generation, maintenance, reuse and validation, we present the Python Vocabulary - a transformative framework designed to support users in managing Python programming. This vocabulary enables users to (1) model and represent Python code constructs, (2) generate and validate Python code programmatically, and (3) ensure adherence to Python syntax and best practices. By leveraging the power of RDF and the flexibility of an abstract syntax tree, the Python Vocabulary enhances the efficiency and reliability of code generation in programming environments, fostering better coding practices and facilitating collaboration in software development projects.

# Audience

This document is intended for a diverse audience of software developers, data scientists, educators, and anyone involved in Python programming and code management. It aims to support users seeking to enhance their understanding and application of Python within the context of semantic technologies and code generation.

# Status

Unstable & unfinished. Work in progress.

# Example - Hello World!

Take for example the following Python code:

```
print("Hello world!")
```

This can be modeled in RDF using the Python Vocabulary:

```
prefix ex: <https://example.org/>
prefix python: <https://www.python.org/model/def/>
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

# Root of the AST (Module node)
ex:module1
  a python:Module ;
  rdf:_1 ex:call1 .

# Call node
ex:call1
  a python:ExpressionCall ;
  rdf:_1 ex:function1 ;
  rdf:_2 ex:args1 .

# Name node (function name 'print')
ex:function1
  a python:ExpressionName ;
  rdf:_1 ex:print1.
  
ex:print1
  a python:SyntaxKeywordPrint.
  
ex:args1
  a python:Args;
  rdf:_1 ex:expressionConstant1.
  
ex:expressionConstant1
  a python:ExpressionConstant;
  rdf:value "Hello world!".  
``` 
 