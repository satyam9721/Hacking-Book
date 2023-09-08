
Many blacklist-based fi lters can be bypassed with almost embarrassing ease 
by making trivial adjustments to the input that is being blocked. For example:
n If SELECT is blocked, try SeLeCt
n If or 1=1-- is blocked, try or 2=2--
n If alert(‘xss’) is blocked, try prompt(‘xss’)


In other cases, fi lters designed to block specifi c keywords can be bypassed by 
using nonstandard characters between expressions to disrupt the tokenizing 
performed by the application. For example:


SELECT/*foo*/username,password/*foo*/FROM/*foo*/users
<img%09onerror=alert(1) src=a>

s, inserting a NULL byte anywhere before a blocked expression 
can cause some fi lters to stop processing the input and therefore not identify 
the expression. For example:

%00<script>alert(1)</script>

from any user-supplied data. However, an attacker may be able to bypass the 
fi lter by supplying the following input:
<scr<script>ipt>

the input is subsequently canonicalized, an attacker may be able to use double 
URL encoding to defeat the fi lter. For example:
%2527
When this input is received, the application server performs its normal URL 
decode, so the input becomes:
%27
This does not contain an apostrophe, so it is permitted by the application’s fi lters. 
But when the application performs a further URL decode, the input is converted 
into an apostrophe, thereby bypassing the fi lter.
If the application strips the apostrophe instead of blocking it, and then performs further canonicalization, the following bypass may be effective:
%%2727
It is worth noting that the multiple validation and canonicalization steps 
in these cases need not all take place on the server side of the application. For 
example, in the following input several characters have been HTML-encoded:
<iframe src=j&#x61;vasc&#x72ipt&#x3a;alert&#x28;1&#x29; 
