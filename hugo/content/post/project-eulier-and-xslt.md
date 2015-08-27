+++
date = "2008-08-22T23:13:00-05:00"
title = "Project Eulier and XSLT"
+++

Well when i came across [Project Euler](http://www.projecteuler.net/) and saw the first problem, i naturally did what anyone in their right mind would do ... solve it using xslt

```
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">
    <xsl:output method="text"/>
    <xsl:variable name="iterations" select="1000"/>
    
    <xsl:template name="sum_multiples">
        <xsl:param name="i">0</xsl:param>
        <xsl:param name="incrementer"></xsl:param>
        <xsl:param name="result">0</xsl:param>
        
        <xsl:choose>
            <xsl:when test="$i < $iterations">
                <xsl:call-template name="sum_multiples">
                    <xsl:with-param name="i" select="$i + $incrementer"/>
                    <xsl:with-param name="result" select="$result + $i"/>
                    <xsl:with-param name="incrementer" select="$incrementer"/>
                </xsl:call-template>
            </xsl:when>
            
            <xsl:otherwise>
                <xsl:value-of select="$result"/>
            </xsl:otherwise>
        </xsl:choose>
    </xsl:template>
    
    <xsl:template match="/">
    <xsl:param name="three">
        <xsl:call-template name="sum_multiples">
            <xsl:with-param name="incrementer">3</xsl:with-param>
        </xsl:call-template>
    </xsl:param>
    <xsl:param name="five">
        <xsl:call-template name="sum_multiples">
            <xsl:with-param name="incrementer">5</xsl:with-param>
        </xsl:call-template>
    </xsl:param>
    <xsl:param name="dups">
        <xsl:call-template name="sum_multiples">
            <xsl:with-param name="incrementer">15</xsl:with-param>
        </xsl:call-template>
    </xsl:param>
    <xsl:value-of select="$three + $five - $dups"/>
    </xsl:template>
</xsl:stylesheet>
```

the same thing in python could go something like

```
three = [x for x in range(1000) if x % 3 is 0]
five = [x for x in range(1000) if x % 5 is 0]
print sum(set(three + five))
```

Edit: Im still learning python, well after posting this and on the ride home i realized i could define my list with one line.

`print sum([x for x in range(1000) if x % 3 is 0 or x % 5 is 0])`
