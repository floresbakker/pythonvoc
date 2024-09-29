# pythonvoc

A RDF-based vocabulary to model Python into RDF. Python logic can thus be represented, queried, generated, validated, analysed, transformed and reused as semantic objects themselves.

# Abstract

The Python Vocabulary provides a formal representation of the Python programming language, enabling the modeling and generation of Python code through an abstract syntax tree (AST) framework. It defines classes and properties to describe the various components of Python syntax, including functions, classes, statements, expressions, and data structures. Additionally, it incorporates SHACL shapes for validating Python code structures and supports algorithms for serializing Python code from RDF representations. This vocabulary enhances code generation and manipulation, bridging Python with broader semantic web technologies.

# Description

The Python Vocabulary formalizes the Python programming language, offering a structured representation of its syntax and semantics. It defines classes for different Python constructs, such as 'python:StatementFunctionDef' for function definitions, 'python:StatementClassDef' for class definitions, and properties to capture relationships between these components. Central to this vocabulary is the class 'python:Module', which serves as a building block for all types of Python code segments. Each code unit can have attributes, such as 'python:decorator' for decorators on functions or classes and 'python:argument' for defining function parameters. The vocabulary also includes SHACL shapes to validate the correctness of Python code structures, ensuring that generated code adheres to Python syntax rules. These shapes facilitate the creation of well-formed Python code that can be executed reliably. This comprehensive approach allows users to leverage RDF representations to generate Python code, enabling seamless integration of semantic technologies with programming tasks.
    
The Python Core Vocabulary models the Python language based on its Abstract Syntax Tree (AST), as shown below:
    
-- ASDL's 4 builtin types are:
-- identifier, int, string, constant

module Python
{
    mod = Module(stmt* body, type_ignore* type_ignores)
        | Interactive(stmt* body)
        | Expression(expr body)
        | FunctionType(expr* argtypes, expr returns)

    stmt = FunctionDef(identifier name, arguments args,
                       stmt* body, expr* decorator_list, expr? returns,
                       string? type_comment, type_param* type_params)
          | AsyncFunctionDef(identifier name, arguments args,
                             stmt* body, expr* decorator_list, expr? returns,
                             string? type_comment, type_param* type_params)

          | ClassDef(identifier name,
             expr* bases,
             keyword* keywords,
             stmt* body,
             expr* decorator_list,
             type_param* type_params)
          | Return(expr? value)

          | Delete(expr* targets)
          | Assign(expr* targets, expr value, string? type_comment)
          | TypeAlias(expr name, type_param* type_params, expr value)
          | AugAssign(expr target, operator op, expr value)
          -- 'simple' indicates that we annotate simple name without parens
          | AnnAssign(expr target, expr annotation, expr? value, int simple)

          -- use 'orelse' because else is a keyword in target languages
          | For(expr target, expr iter, stmt* body, stmt* orelse, string? type_comment)
          | AsyncFor(expr target, expr iter, stmt* body, stmt* orelse, string? type_comment)
          | While(expr test, stmt* body, stmt* orelse)
          | If(expr test, stmt* body, stmt* orelse)
          | With(withitem* items, stmt* body, string? type_comment)
          | AsyncWith(withitem* items, stmt* body, string? type_comment)

          | Match(expr subject, match_case* cases)

          | Raise(expr? exc, expr? cause)
          | Try(stmt* body, excepthandler* handlers, stmt* orelse, stmt* finalbody)
          | TryStar(stmt* body, excepthandler* handlers, stmt* orelse, stmt* finalbody)
          | Assert(expr test, expr? msg)

          | Import(alias* names)
          | ImportFrom(identifier? module, alias* names, int? level)

          | Global(identifier* names)
          | Nonlocal(identifier* names)
          | Expr(expr value)
          | Pass | Break | Continue

          -- col_offset is the byte offset in the utf8 string the parser uses
          attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

          -- BoolOp() can use left & right?
    expr = BoolOp(boolop op, expr* values)
         | NamedExpr(expr target, expr value)
         | BinOp(expr left, operator op, expr right)
         | UnaryOp(unaryop op, expr operand)
         | Lambda(arguments args, expr body)
         | IfExp(expr test, expr body, expr orelse)
         | Dict(expr* keys, expr* values)
         | Set(expr* elts)
         | ListComp(expr elt, comprehension* generators)
         | SetComp(expr elt, comprehension* generators)
         | DictComp(expr key, expr value, comprehension* generators)
         | GeneratorExp(expr elt, comprehension* generators)
         -- the grammar constrains where yield expressions can occur
         | Await(expr value)
         | Yield(expr? value)
         | YieldFrom(expr value)
         -- need sequences for compare to distinguish between
         -- x < 4 < 3 and (x < 4) < 3
         | Compare(expr left, cmpop* ops, expr* comparators)
         | Call(expr func, expr* args, keyword* keywords)
         | FormattedValue(expr value, int conversion, expr? format_spec)
         | JoinedStr(expr* values)
         | Constant(constant value, string? kind)

         -- the following expression can appear in assignment context
         | Attribute(expr value, identifier attr, expr_context ctx)
         | Subscript(expr value, expr slice, expr_context ctx)
         | Starred(expr value, expr_context ctx)
         | Name(identifier id, expr_context ctx)
         | List(expr* elts, expr_context ctx)
         | Tuple(expr* elts, expr_context ctx)

         -- can appear only in Subscript
         | Slice(expr? lower, expr? upper, expr? step)

          -- col_offset is the byte offset in the utf8 string the parser uses
          attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

    expr_context = Load | Store | Del

    boolop = And | Or

    operator = Add | Sub | Mult | MatMult | Div | Mod | Pow | LShift
                 | RShift | BitOr | BitXor | BitAnd | FloorDiv

    unaryop = Invert | Not | UAdd | USub

    cmpop = Eq | NotEq | Lt | LtE | Gt | GtE | Is | IsNot | In | NotIn

    comprehension = (expr target, expr iter, expr* ifs, int is_async)

    excepthandler = ExceptHandler(expr? type, identifier? name, stmt* body)
                    attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

    arguments = (arg* posonlyargs, arg* args, arg? vararg, arg* kwonlyargs,
                 expr* kw_defaults, arg? kwarg, expr* defaults)

    arg = (identifier arg, expr? annotation, string? type_comment)
           attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

    -- keyword arguments supplied to call (NULL identifier for **kwargs)
    keyword = (identifier? arg, expr value)
               attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

    -- import name with optional 'as' alias.
    alias = (identifier name, identifier? asname)
             attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

    withitem = (expr context_expr, expr? optional_vars)

    match_case = (pattern pattern, expr? guard, stmt* body)

    pattern = MatchValue(expr value)
            | MatchSingleton(constant value)
            | MatchSequence(pattern* patterns)
            | MatchMapping(expr* keys, pattern* patterns, identifier? rest)
            | MatchClass(expr cls, pattern* patterns, identifier* kwd_attrs, pattern* kwd_patterns)

            | MatchStar(identifier? name)
            -- The optional "rest" MatchMapping parameter handles capturing extra mapping keys

            | MatchAs(pattern? pattern, identifier? name)
            | MatchOr(pattern* patterns)

             attributes (int lineno, int col_offset, int end_lineno, int end_col_offset)

    type_ignore = TypeIgnore(int lineno, string tag)

    type_param = TypeVar(identifier name, expr? bound)
               | ParamSpec(identifier name)
               | TypeVarTuple(identifier name)
               attributes (int lineno, int col_offset, int end_lineno, int end_col_offset)
}

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

print("Hello world!")

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
ex:Call1
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
 