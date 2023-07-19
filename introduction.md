
# Introduction

myMVC is a **PHP** framework for running on **Linux**, with which great things can be realized. Be it frontends, backends, APIs or a mix of all.

With myMVC I have used, incorporated and implemented some views, best practices and insights that have become important to me in the course of a developer's life.
and which have ultimately led to the fact that myMVC perhaps also stands out a bit from other frameworks. These things I want to show here once.


## You develop your code in modules

With myMVC, the code you write as a developer is separate from the framework itself.
It is not stored in various folders within the downloaded framework - as is the case with other frameworks where you have to store your code fragmented in many different places.

Instead, you write all your code in a single, separate folder -a module- under `/modules/{myModule}/` - completely separate from the framework code and its folder structure.

Advantage:
- Your module as a whole can be easily uploaded to a separate git repository.
- You don't need to version the whole framework itself, only your module is enough.
- Your module is relatively easy to adapt to newer myMVC versions - copy the module and adapt it.

Mnemonics:
- You only work in the module
- You just don't touch the framework around it


## Separation of public and private

Only what is in the folder `/public` is public.  
Everything else is per se non-public and not accessible from outside.   
About bootstrapping `/public/index.php` the whole application is loaded.


## The framework does not have to be able to do everything

myMVC should be able to do basic things like routing but not also generate PDF.  
This will not happen with myMVC.  
Thus the framework remains relatively slim by simply limiting itself to the most important things.


## Clean Code Development

I am a friend of clean code and therefore I try to provide clean code as well. Maybe you will notice that I name variables in a certain way.
Basically: A variable is named according to its type or content or usage if possible.

_Examples for naming variables_    
~~~php
// leading $b for boolean type
$bSuccess = true;

// leading $i for integer type
$iYear = 1969;

// leading $s for type string
$sName = 'Foo Bar';

// leading $a for array type
$aData = array(1, 2, 3);

// leading $o for type object
$oDTRequestCurrent = \MVC\Request::getCurrentRequest();

// leading $r or $h for type resource|handle
$rCurl = curl_init();
$hCurl = curl_init();

// leading $f|$d for float (double) type
$fPrice = 10.90;
$dPrice = 10.90;

// leading $m for mixed data
$mData;
~~~

---

become curious ? Then check out the next chapter <a class="btn btn-info" href="/3.3.x/installation"> >> Installation </a>









