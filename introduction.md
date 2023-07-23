
# Introduction

**myMVC is a `PHP framework` for running on Linux, with which great things can be realized. Be it frontends, backends, APIs or a mix of all.**

There are of course infinite ways to write code and also to build a framework. And as so often in life, there is no unrestricted right or wrong, but only preferences and taste.
So it is also with this framework; It is based on experience, views, adopted best practices and knowledge, but sometimes also on deliberate abandonment of things.

**Linux**  
The framework is made to run on Linux.

**You develop your code in modules**  
With myMVC, the code you write as a developer is separate from the framework itself.
It is not stored in various folders within the downloaded framework - as is the case with other frameworks where you have to store your code fragmented in many different places.

Instead, you write all your code in a single, separate folder -a module- under `/modules/{myModule}/` - completely separate from the framework code and its folder structure.

Advantages are: (a) Your module as a whole can be easily uploaded to a separate git repository.
(b) You don't need to version the whole framework itself, only your module is enough. (c) Your module is relatively easy to adapt to newer myMVC versions - copy the module and adapt it.

Rules of thumb:
- You only work in the module
- You just don't touch the framework around it

**Strict Separation of public and private**  
Only what is in the folder `/public` is public. Everything else is per se non-public and not accessible from outside. 
About bootstrapping `/public/index.php` the whole application is loaded.

**The framework does not have to be able to do everything**  
myMVC should be able to do basic things like routing but not also generate PDF. This will not happen with myMVC. 
Thus the framework remains relatively slim by simply limiting itself to the most important things. 
Instead, extend your application code by the libraries of your favor if you need some.

**Clean Code Development**  
myMVC loves clean code, and therefore it tries to provide clean code under the hood as well following Clean Code concepts like <a href="https://www.php-fig.org/psr/psr-2/" target="_blank">PSR-2</a>, <a href="https://www.php-fig.org/psr/psr-12/" target="_blank">PSR-12</a>
but also <a href="https://blog.ueffing.net/post/2023/07/21/php-better-way-to-name-variables-because-you-want-to-be-understood/" target="_blank">php: better way to name variables - because you want to be understood</a>

---

**"Why another Framework - there are many, even better ones ?" you might ask.**  

_Sure, there are many out there and most of them are by far more popular than this one. It is small, unknown and thus does not have any worth mentioning community.
But maybe one or the other finds this framework quite good and decides to work with it - that would make me happy.
In any case, I don't care and I love developing and working with myMVC.  
Yours, G. Üffing_
