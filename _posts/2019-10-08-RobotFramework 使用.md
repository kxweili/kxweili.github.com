---
layout: post
title: RobotFramework quick use
category:  Framework
---

+  [Robot framework user tutorial](http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html)   
+  [Robot framework introduction](https://robotframework.org)     
+  [Public API](https://robot-framework.readthedocs.io/en/latest/)    

Here's simple demo
```
*** Settings ***
Documentation  present some info of this test suite
Library  SeleniumLibrary

*** Variables ***
${Browser}  chrome

*** Test Cases ***
Guest must sign in to check
    [Documentation]  test this
    Open Browser  http://www.amazon.com  ${Browser}
    Close Browser
```
You can install the Library using    
```
pip3 install robotframework-seleniumLibrary
```
