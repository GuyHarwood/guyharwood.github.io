---
title: Solution review and refactoring with NDepend and Resharper
slug: solution_review_and_refactoring_with_ndepend_and_resharper
date_published: 2013-02-10T13:11:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: NDepend, Refactoring, Architecture
---

Recently I have been performing a lot of refactoring on a commercial web product that I built over 6 years ago.  It’s got a bit of everything in there - good code, bad code, the "please just work so I can go to bed" quick fixes and everything else in between.

My idea of good software has changed significantly in the last 6 years, and I didn’t take unit testing seriously back then.  Suffice to say, efforts to introduce long term improvements to the stability of this product are not going to be successful without some significant refactoring to the overall structure of the solution.

### Tools for the job

Resharper is really good at refactoring code in a safe and speedy fashion, but I needed to go a little further and look at ways that I could improve the overall architecture of the solution.  I’ve been using [NDepend](http://www.ndepend.com/) for this on more recent projects, so I fired it up and instructed it to scan the key assemblies within the solution.  The initial report that NDepend presents to you once it’s completed the scan be pretty overwhelming at first.  It contains an extremely comprehensive view of your solution and potential problems within.  To get an idea of the kind of information it can provide you with, you can check out some [sample reports on the NDepend website](http://www.ndepend.com/SampleReports.aspx)[![](http://2.bp.blogspot.com/-i0H1OgboTkg/URecLjkMQWI/AAAAAAAAA94/Mrbet8c4dTE/s640/NDependBig09.png)](http://2.bp.blogspot.com/-i0H1OgboTkg/URecLjkMQWI/AAAAAAAAA94/Mrbet8c4dTE/s1600/NDependBig09.png)

### Querying your code

The code query tool allows you to pose questions about your code, and get immediate results.  It’s like a traditional query analyser – and the database is your code.  There are so many things you can do with this tool, and to get you started NDepend comes preloaded with a solid set of queries that cover a lot of the aspects you should be concerned about.  It uses LINQ syntax, so you feel right at home when constructing result sets of interest.

Methods with too many parameters?  You can seek out offending methods by simply changing a parameter on one of the existing queries…

[![](http://2.bp.blogspot.com/-2d1FoVGSuNY/URf2ArX6e0I/AAAAAAAAA-s/0Mj-xMuoxqY/s640/tooManyParams+(1).png)](http://2.bp.blogspot.com/-2d1FoVGSuNY/URf2ArX6e0I/AAAAAAAAA-s/0Mj-xMuoxqY/s1600/tooManyParams+(1).png)

Want to ensure your code is in line with the SOLID principles?  We can easily find out if we have violated the Interface segregation principle by crafting a query to find interfaces with many members that potentially have cross cutting concerns…

![](http://3.bp.blogspot.com/-WuZpa0kbQII/UReckPaI-FI/AAAAAAAAA-A/HZKP-pV53kg/s1600/InterfaceSegregation.png)

Clicking on the Type listed in the results pane will take you straight to the code in question, at which point you can break out some Resharper tricks to finish off the job in a safe and structured manner.

The editor has built-in intellisense, so working up your own queries is a pretty trivial affair.  I have only had to create one or two queries of my own, as most of the issues of interest to me are covered in the queries provided out of the box.  The intellisense is decent (although doesn’t like the tab key for completion), and provides you with contextual information along the way…

[![](http://1.bp.blogspot.com/-lFx2Sr0L7Q4/URectRF9I_I/AAAAAAAAA-I/HHIidPfoZKU/s640/CodeQueryTool-TooltipsAndAutoComplete.png)](http://1.bp.blogspot.com/-lFx2Sr0L7Q4/URectRF9I_I/AAAAAAAAA-I/HHIidPfoZKU/s1600/CodeQueryTool-TooltipsAndAutoComplete.png)

One of the most useful queries, has to be the ‘Summary of methods to refactor’ query.  It takes some key method metrics into consideration – lines of code, cyclomatic complexity, number of parameters, number of variables and amount of overloads…

![](http://3.bp.blogspot.com/-DsjjyyP_GWY/UReczGeF0QI/AAAAAAAAA-Q/fiS090Gvpko/s1600/Query+for+code+that+needs+refactoring.png)

If any of the criteria aren’t obvious to you, the NDepend team have very helpfully provided links to those specific topics within the product website.  So you may well walk away from an NDepend session understanding more about writing robust code.  Taking it further, there is a comprehensive help section available too…

![](http://1.bp.blogspot.com/-CKL-4zFVssA/URec2j8OkdI/AAAAAAAAA-Y/mJET0mKyBh4/s1600/helpNDepend.png)

Something that I haven’t got my head around yet, but is firmly near the top of my TODO list, is [integrating NDepend into the CI pipeline](http://www.ndepend.com/Features.aspx#BuildProcess) to quickly and easily identify issues with code throughout the development life cycle.  Powerful stuff.

### Conclusion

Refactoring legacy code can be a cumbersome task, but I can safely say it’s something of a creative and productive experience with NDepend and Resharper at my disposal.

Productivity tools aside - If you find yourself working with legacy code / existing applications quite a lot, I can whole heartedly recommend picking up the [Brownfield Application Development book](https://www.manning.com/books/brownfield-application-development-in-dot-net) written by Kyle Baley and Donald Belcham.  I found myself smiling while reading certain sections, relating my own experiences to those of the authors, and it really does help to underline and define some of the problems and proven techniques for solving them within existing codebases.
