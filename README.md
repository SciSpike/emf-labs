# Eclipse Modeling Framework

## Welcome

Welcome to this course on the Eclipse Modeling Framework (EMF).

This is a hands-on course that teaches you how to use EMF for various use-cases.
We will be using the latest version of Eclipse and EMF throughout the course.

## Slides

The slides are available for download.
The instructor will provide you with a URL from which you can download the slides.

## Labs

This repository contains all the labs.
The labs are provided in Markdown files.
Markdown files are text files with various annotations to help render the files.
To render the files nicely, you will have to use some kind of Markdown reader.

You have a couple of options:

1. Read the files on GitHub
2. Download a markdown reader/tool
3. Install a markdown plugnin in Google Chrome and render them locally

For now, we'll just assume that you are reading this file on GitHuib and in that case, all the links to the labs below should work.

## Outline of Course

* Chapter 1: Course Overview
  * Introduction
  * [Lab: Install Eclipse (if you have not already done so)](labs/00_installation.md)
* Chapter 2: Introduction to EMF
  * Eclipse Basics
  * A first example of an Eclipse Plugin and a typical Eclipse Plugin Architecture
  * [Lab: Create an address book editor](labs/02_AddressBook.md)
  * EMF Overview
* Chapter 3: Model Driven Development (MDD)
  * MDD overview
  * Meta-modeling
* Chapter 4: The Ecore Model
  * Purpose of Ecore
  * Different ways to create ecore models
  * [Lab: Create an ecore model using the built in ecore editor](labs/03_MusicLibraryWithEcore.md)
* Chapter 5: The Generator Model
  * The role of the GenModel
  * Configuration of the GenModel
  * [Lab: Create a genmodel for the music library](labs/04_Genmodel.md)
* Chapter 6: Code Generation
  * What is generated?
  * What is not generated?
  * Overriding generated code
  * Java Emitter Template (JET)
  * Other options for code generation
  * [Lab: Generate code for the music library](labs/05_ModifyGenModel.md)
* Chapter 7: EMF.model
  * What is the EMF.model?
  * EMF.model dependencies
  * The anatomy of the EMF.model
  * [Lab: Operations](labs/06_Operation.md)
* Chapter 8: EMF.edit
  * The role of EMF.edit
  * What is generated?
  * EMF.edit and design patterns
  * Modifying presentation behavior
  * Modifying commands
  * [Lab: Modify the label provider](labs/07_ModifyLabelProvider.md)
* Chapter 9: EMF.editor
  * Role of the EMF.editor
  * What is generated?
  * Graphical Modeling Framework
* Chapter 10: EMF Client Programming
  * Accessing the business tier
  * Creating objects
  * Accessing resources
  * Using adapters
  * [Lab: Use the generated code](labs/08_UseGeneratedCode.md)
  * [Lab: Use EMF](labs/09_use-emf.md)
  * [Lab: Reflective Operations](labs/10_reflective-operations.md)
  * [Lab: Deeper look at reflection](labs/11_Reflective2.md)
  * [Lab: Using adapters](labs/12_adapters.md)
  * [Lab: Using JUnit with EMF](labs/13_junit.md)
