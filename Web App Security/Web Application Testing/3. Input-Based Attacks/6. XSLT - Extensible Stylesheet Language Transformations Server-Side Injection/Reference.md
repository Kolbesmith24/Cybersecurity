# XSLT
[eXtensible Stylesheet Language Transformation (XSLT)](https://www.w3.org/TR/xslt-30/) is a language enabling the transformation of XML documents. For instance, it can select specific nodes from an XML document and change the XML structure.

XSLT (Extensible Stylesheet Language Transformations) server-side injection is a vulnerability that arises when an attacker can manipulate XSLT transformations performed on the server. 

XSLT is a language used to transform XML documents into other formats, such as HTML, and is commonly employed in web applications to generate content dynamically. 

In the context of XSLT server-side injection, attackers exploit weaknesses in how XSLT transformations are handled, allowing them to inject and execute arbitrary code on the server.
## eXtensible Stylesheet Language Transformation (XSLT)
XSLT can be used to define a data format which is subsequently enriched with data from an originating XML document.

XSLT data is structured similarly to XML. However, it contains XSL elements within nodes prefixed with the `xsl`-prefix. The following are some commonly used XSL elements:

- `<xsl:template>`: This element indicates an XSL template. It can contain a `match` attribute that contains a path in the XML document that the template applies to
- `<xsl:value-of>`: This element extracts the value of the XML node specified in the `select` attribute
- `<xsl:for-each>`: This element enables looping over all XML nodes specified in the `select` attribute
- `<xsl:sort>`: This element specifies how to sort elements in a for loop in the `select` argument. Additionally, a sort order may be specified in the `order` argument
- `<xsl:if>`: This element can be used to test for conditions on a node. The condition is specified in the `test` argument.
# XSLT Injection
XSLT injection occurs whenever user input is inserted into XSL data before output generation by the XSLT processor. This enables an attacker to inject additional XSL elements into the XSL data, which the XSLT processor will execute during output generation.
## Exploiting XSLT Injection
### Identifying XSLT Injection
If we find an input that is reflected on the page (such as our name), and we can suppose the web application stores the module information in an XML document and displays the data using XSLT processing.
- In that case, it might suffer from XSLT injection if our name is inserted without sanitization before XSLT processing.

To confirm that, let us try to inject a broken XML tag to try to provoke an error in the web application. We can achieve this by providing the username `<`:
![](https://academy.hackthebox.com/storage/modules/145/xslt/xslt_exploitation_3.png)
- The web application responds with a server error (500)
- This does not confirm that an XSLT injection vulnerability is present, it might indicate the presence of a security issue.
### Information Disclosure
We can try to infer some basic information about the XSLT processor in use by injecting the following XSLT elements:
```xml
Version: <xsl:value-of select="system-property('xsl:version')" />
<br/>
Vendor: <xsl:value-of select="system-property('xsl:vendor')" />
<br/>
Vendor URL: <xsl:value-of select="system-property('xsl:vendor-url')" />
<br/>
Product Name: <xsl:value-of select="system-property('xsl:product-name')" />
<br/>
Product Version: <xsl:value-of select="system-property('xsl:product-version')" />
```

If the web application interpreted the XSLT elements we provided, this confirms an XSLT injection vulnerability.
- Furthermore, we can deduce the library and XSLT version from this.
### Local File Inclusion (LFI)
We can try to use multiple different functions to read a local file. Whether a payload will work depends on the XSLT version and the configuration of the XSLT library.

For instance, XSLT contains a function `unparsed-text` that can be used to read a local file:
```xml
<xsl:value-of select="unparsed-text('/etc/passwd', 'utf-8')" />
```
- However, it was only introduced in XSLT version 2.0. Thus, some web applications may not support this function and instead error it out.

If the XSLT library is configured to support PHP functions, we can call the PHP function `file_get_contents` using the following XSLT element:
```xml
<xsl:value-of select="php:function('file_get_contents','/etc/passwd')" />
```
### Remote Code Execution (RCE)
If an XSLT processor supports PHP functions, we can call a PHP function that executes a local system command to obtain RCE. 

For instance, we can call the PHP function `system` to execute a command:
```xml
<xsl:value-of select="php:function('system','id')" />
```