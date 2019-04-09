# Chapter 6. Framework Fundamentals
* String Trivia:
    * Ordinal Versus Culture Comparision - Ordinal uses ASCII (A...Z...a...z) while Culture uses alphabet (AaBb...Zz)
    * StringBuilder caveat - Setting a StringBuilder's length to 0 does not change its _internal_ capacity. To free the space it previously occupied, simply create a new StringBuilder and drops the old one out of the scope to be GC.