# Cloudflare API | DNS Records
I’m probably doing Cloudflare a disservice by categorising it as a CDN provider, but that’s certainly one of the many services they offer and perhaps how most individuals using their free offering see them. Like me, I’m certain the vast majority of that group use the Cloudflare dashboard to configure their domains, but Cloudflare provides an API that allows you to programmatically manage those DNS records through a command-line interface of a \*nix shell such as Bash.

The documentation states:

> Using Cloudflare’s API, you can do just about anything you can do on cloudflare.com via the customer dashboard.

Although the Cloudflare API [documentation](https://api.cloudflare.com/) gives numerous examples, it took me a while to get to grips with them, so I thought it may be useful for others if I document my examples.

To use the Cloudflare API you’ll need the email address used to login to your Cloudflare account and your Cloudlare account’s [global API key](https://support.cloudflare.com/hc/en-us/articles/200167836-Where-do-I-find-my-Cloudflare-API-key-).

### 2. ABOUT THE CODE SAMPLES

### 2.1 Code

Below is an example of code used throughout this article together with the output if the request is successful and the output if unsuccessful:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="example.com"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

I’ve assigned certain values in the code to variables – `EMAIL`, `KEY` etc – to make it easier to see how this data is used throughout the code. Below is a sample DNS record on the Cloudflare customer dashboard together with a table summarising the variables used.

[![](https://www.tech-otaku.com/wp-content/uploads/4592-04-cloudflare-dashboard-showing-domains-dns-record.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-04-cloudflare-dashboard-showing-domains-dns-record.png)

Cloudflare dashboard showing a single DNS record for example.com

| Variable | Value |
\| `EMAIL` | The email address associated with your Cloudflare account. |
\| `KEY` | The global API key associated with your Cloudflare account. |
\| `TOKEN` | Similar to an API key, but allows more fine-tuned access. |
\| `DOMAIN` | The name of the domain to create a zone record for. |
\| `JUMP_START` | If true, automatically attempts to fetch existing DNS records when creating a domain’s zone record |
\| `ZONE_ID` | The unique ID of the domain’s zone record. Assigned by Cloudflare. Required when managing an existing zone record and its DNS records. |
\| `DNS_ID` | The unique ID given to each of the domain’s individual DNS records. Assigned by Cloudflare. Required when updating or deleting an existing DNS record. |
\| `TYPE` | The DNS record type including A, CNAME, MX and TXT records. This equates to the _**Type**_ column on the Cloudflare dashboard. |
\| `NAME` | The DNS record name. This equates to the _**Name**_ column on the Cloudflare dashboard. |
\| `CONTENT` | The DNS record content. This equates to the _**Value**_ column on the Cloudflare dashboard. |
\| `PROXIED` | If true, a DNS record will pass through Cloudflare’s servers. Un-proxied records will not and are for DNS resolution only. Applicable to A and CNAME records only. This equates to the _**Status**_ column on the Cloudflare dashboard. |
\| `TTL` | Valid TTL. Must be between 120 and 2,147,483,647 seconds, or 1 for automatic |
\| `PRIORITY` | The order in which servers should be contacted. Applicable to MX records only. |
\| `ALL` | If true, JSON output will be pretty-printed using Python’s json.tool module. Otherwise, output will be limited to specified data. |

Variables Passed To The Cloudflare API

### 2.2 Output

All requests to Cloudflare’s API are made using HTTPS and return unformatted JSON data. To make this output readable, I’ve piped the JSON data through Python’s `json.tool` – a simple command line interface for the json module to pretty-print JSON objects. All key/value pairs are sorted alphabetically.

HTTPS requests to the API using the `POST` or `DELETE` methods return a JSON object containing 4 top-level key/value pairs. These keys are `errors`, `messages`, `result` and `success`. The values for these keys can be an empty array:

…another object containing other key/value pairs:

```"result"``: {```

```"content"``:``` ```"203.0.113.50"``,```

```"created_on"``:``` ```"2018-01-27T15:57:52.254408Z"``,```

`...`

`},`

…or a boolean value:

For HTTPS requests using `GET`, `result` key value objects are contained within an array:

```"result"``: [```

`{`

`...`

```"id"``:``` ```"8b717207bcee4047af2e9dff95832996"``,```

`...`

`}`

`],`

In addition there is a fifth top-level key/value pair for the `GET` method named `result_info` whose value is an object containing other key/value pairs:

```"result_info"``: {```

```"count"``:``` ```1``,```

`...`

`},`

### 2.3 Alternative Code and Output

Some requests to the Cloudflare API produce a lot of JSON data. As an alternative to the main code samples, I have provided alternative code whose output focuses on a particular piece of data – the unique ID of a domain’s DNS record for example. In the example below, the variable `ALL` is included and controls how JSON data is displayed. If `ALL` is `true`, JSON data is piped to Python’s `json.tool` as before. However, if `false`, only limited JSON data – as specified in the statements passed as a command to `python` – is displayed.

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="example.com"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
ALL="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('Type: ' + data\['result']\['type'] + '\\n'+'DNS ID: ' + data\['result']\['id'] if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

### 2.4 API Keys vs API Tokens

Cloudflare are moving away from using legacy [API keys](https://developers.cloudflare.com/api/keys) to [API tokens](https://developers.cloudflare.com/api/tokens). All of the examples in this article use an API key to access the Cloudflare API and continue to work – for now. To use an API token, you need to change this example code…

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="example.com"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE\\\_ID/dns\\\_records/](https://api.cloudflare.com/client/v4/zones/$ZONE\_ID/dns\_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

…to this…

TOKEN="ZsJcjFDfHxaVnEjjfDdvgpFDHF6jsUGVvyzd6M8R"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="example.com"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE\\\_ID/dns\\\_records/](https://api.cloudflare.com/client/v4/zones/$ZONE\_ID/dns\_records/)" \\
    \-H "Authorization: Bearer $TOKEN" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

### 2.5 Automated Shell Script

I found working with the Cloudflare API cumbersome and somewhat error prone and so put together a shell script to streamline adding new DNS records and updating and deleting existing DNS records. The script requires the domain to have an existing zone record and allows this…

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="example.com"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE\\\_ID/dns\\\_records/](https://api.cloudflare.com/client/v4/zones/$ZONE\_ID/dns\_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

…to be replaced with:

./cf-dns.sh -d example.com -n example.com -c 203.0.113.50 -l 1 -x y -k

Much simpler and no need to be concerned with getting zone IDs or DNS record IDs. The GitHub repository with documentation and examples can be found at [cloudflare-dns](https://github.com/tech-otaku/cloudflare-dns).

### 3. ClOUDFLARE RESTRICTIONS

Adding DNS records on Cloudflare for a given domain will have no affect on where that domain’s DNS resolves until its nameservers are changed via the domain’s registrar to point to Cloudflare’s. As such, you should be able to use any domain name when experimenting with the Cloudflare API, but Cloudflare does impose some restrictions:

-   You can’t create DNS records for recognised domains like google.com, microsoft.com, example.com etc. Should you try, you’ll receive the error: _This zone is banned and cannot be added to Cloudflare at this time_. I’ve used example.com in all of my code examples, but this is purely illustrative.
-   The domain must be registered. You can’t use google.test, microsoft.invalid, example.localhost even though these TLDs have been reserved for testing and documentation purposes by the IETF ([RFC 2606](https://tools.ietf.org/pdf/rfc2606.pdf)). The error message is _We were unable to identify example.localhost as a registered domain_.
-   A valid IP address is also required. Using 12.345.678.9 returns _DNS Validation Error_. I used 203.0.113.50 which is in a range of IP addresses which the IETF ([RFC 5737](https://tools.ietf.org/pdf/rfc5737.pdf)) have reserved for testing and documentation purposes.

### 4. WORKING CODE EXAMPLES

Below is a screenshot of the Cloudflare dashboard showing DNS records for the domain example.com. We’ll create each of these DNS records using the Cloudflare API.

[![](https://www.tech-otaku.com/wp-content/uploads/4592-05-cloudflare-dashboard-showing-dns-records-for-dummy-com.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-05-cloudflare-dashboard-showing-dns-records-for-dummy-com.png)

Cloudflare dashboard showing DNS records for example.com

Each of the following code examples when executed from the command-line should do exactly as described, providing valid data is supplied for `EMAIL`, `KEY` and where applicable `DOMAIN`, `ZONE_ID` and `DNS_ID`.

All code examples were successfully tested using cURL 7.54.0 and Python 2.7.10 on macOS High Sierra 10.13.3 and cURL 7.47.0 and Python 2.7.12 on Ubuntu 16.04.3.

### 4.1 ZONE RECORDS

### 4.1.1 List All Zone Records

Many interactions with the Cloudflare API require the domain’s zone ID. To list all the domains on your Cloudflare account together with their individual zone ID, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
curl -X GET "[https://api.cloudflare.com/client/v4/zones](https://api.cloudflare.com/client/v4/zones)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | python -c $'import sys,json\\ndata=json.loads(sys.stdin.read())\\nif data\["success"]:\\n\\tfor dict in data\["result"]:print("Zone ID:" + dict\["id"] + ", Domain:" + dict\["name"])\\nelse:print("ERROR(" + str(data\["errors"]\[0]\["code"]) + "):" + data\["errors"]\[0]\["message"])'

### 4.1.2 Create a New Zone Record for a Domain

In order to create DNS records for a domain, we first need to create a unique zone record for that domain to which we’ll later add these DNS records. To create a zone record for `example.com`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
DOMAIN="example.com"; \\
JUMP_START="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/](https://api.cloudflare.com/client/v4/zones/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"name":"'"$DOMAIN"'","jump_start":'"$JUMP_START"'}' \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
DOMAIN="example.com"; \\
JUMP_START="false"; \\
ALL="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/](https://api.cloudflare.com/client/v4/zones/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"name":"'"$DOMAIN"'","jump_start":'"$JUMP_START"'}' \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('ZONE_ID: ' + data\['result']\['id'] if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

### 4.1.3 List an Existing Zone Record for a Domain

To display the existing zone record for `example.com`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
DOMAIN="example.com"; \\
curl -X GET "[https://api.cloudflare.com/client/v4/zones?name=$DOMAIN](https://api.cloudflare.com/client/v4/zones?name=$DOMAIN)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
DOMAIN="example.com"; \\
ALL="false"; \\
curl -X GET "[https://api.cloudflare.com/client/v4/zones?name=$DOMAIN](https://api.cloudflare.com/client/v4/zones?name=$DOMAIN)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('ZONE_ID: ' + data\['result']\[0]\['id'] if data\['result'] else 'ERROR: Does a zone record for \\"$DOMAIN\\"exist?')"; fi

### 4.1.4 Delete an Existing Zone Record for a Domain

To delete the existing zone record for `example.com` and all its related DNS records, use the following code. Note that we need to provide the unique ID of the domain’s existing zone record:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
curl -X DELETE "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID](https://api.cloudflare.com/client/v4/zones/$ZONE_ID)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
ALL="false"; \\
curl -X DELETE "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID](https://api.cloudflare.com/client/v4/zones/$ZONE_ID)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('Zone for ID \\"$ZONE_ID\\"deleted.' if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

### 4.2 CREATE NEW DNS RECORDS

### 4.2.1 Create a New DNS \[A] Record for a Domain

To create a DNS record that points `example.com` to the IP address `203.0.113.50`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="example.com"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="example.com"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
ALL="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('Type: ' + data\['result']\['type'] + '\\n'+'DNS ID: ' + data\['result']\['id'] if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

The Cloudflare dashboard now shows:

[![](https://www.tech-otaku.com/wp-content/uploads/4592-04-cloudflare-dashboard-showing-domains-dns-record.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-04-cloudflare-dashboard-showing-domains-dns-record.png)

Cloudflare dashboard showing the newly created **A** record for example.com

### 4.2.2 Create a New DNS \[A] Record for a Sub-domain

To create a DNS record that points `sub-domain.example.com` to the IP address `203.0.113.50`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="sub-domain"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="A"; \\
NAME="sub-domain"; \\
CONTENT="203.0.113.50"; \\
PROXIED="true"; \\
TTL="1"; \\
ALL="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('Type: ' + data\['result']\['type'] + '\\n'+'DNS ID: ' + data\['result']\['id'] if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

The Cloudflare dashboard now shows:

[![](https://www.tech-otaku.com/wp-content/uploads/4592-06-cloudflare-dashboard-showing-2-dns-records-for-dummy-com.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-06-cloudflare-dashboard-showing-2-dns-records-for-dummy-com.png)

Cloudflare dashboard showing the newly created **A** record for sub-domain.example.com

### 4.2.3 Create a New DNS \[CNAME] Record for an Alias

To create a DNS record that makes `www.example.com` an alias of `example.com`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="CNAME"; \\
NAME="www"; \\
CONTENT="example.com"; \\
PROXIED="true"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="CNAME"; \\
NAME="www"; \\
CONTENT="example.com"; \\
PROXIED="true"; \\
TTL="1"; \\
ALL="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('Type: ' + data\['result']\['type'] + '\\n'+'DNS ID: ' + data\['result']\['id'] if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

The Cloudflare dashboard now shows:

[![](https://www.tech-otaku.com/wp-content/uploads/4592-07-cloudflare-dashboard-showing-3-dns-records-for-dummy-com.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-07-cloudflare-dashboard-showing-3-dns-records-for-dummy-com.png)

Cloudflare dashboard showing the newly created **CNAME** record for www.example.com

### 4.2.4 Create a New DNS \[MX] Record for a Domain

To create a DNS record that specifies the primary mail server to handle mail for `example.com`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="MX"; \\
NAME="@"; \\
CONTENT="aspmx.l.google.com"; \\
PRIORITY="1"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","priority":'"$PRIORITY"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="MX"; \\
NAME="@"; \\
CONTENT="aspmx.l.google.com"; \\
PRIORITY="1"; \\
TTL="1"; \\
ALL="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","priority":'"$PRIORITY"',"ttl":'"$TTL"'}' \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('Type: ' + data\['result']\['type'] + '\\n'+'DNS ID: ' + data\['result']\['id'] if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

The Cloudflare dashboard now shows:

[![](https://www.tech-otaku.com/wp-content/uploads/4592-08-cloudflare-dashboard-showing-4-dns-records-for-dummy-com.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-08-cloudflare-dashboard-showing-4-dns-records-for-dummy-com.png)

Cloudflare dashboard showing the newly created 1st **MX** record for example.com

### 4.2.5 Create a Second New DNS \[MX] Record for a Domain

To create a DNS record that specifies the secondary mail server to handle mail for `example.com`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="MX"; \\
NAME="@"; \\
CONTENT="alt1.aspmx.l.google.com"; \\
PRIORITY="5"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","priority":'"$PRIORITY"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="MX"; \\
NAME="@"; \\
CONTENT="alt1.aspmx.l.google.com"; \\
PRIORITY="5"; \\
TTL="1"; \\
ALL="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","priority":'"$PRIORITY"',"ttl":'"$TTL"'}' \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('Type: ' + data\['result']\['type'] + '\\n'+'DNS ID: ' + data\['result']\['id'] if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

The Cloudflare dashboard now shows:

[![](https://www.tech-otaku.com/wp-content/uploads/4592-09-cloudflare-dashboard-showing-5-dns-records-for-dummy-com.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-09-cloudflare-dashboard-showing-5-dns-records-for-dummy-com.png)

Cloudflare dashboard showing the newly created 2nd **MX** record for example.com

### 4.2.6 Create a New DNS \[TXT] Record

To create a DNS record that specifies the sender policy framework (SPF) for `example.com`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="TXT"; \\
NAME="@"; \\
CONTENT="v=spf1 include:\_spf.google.com ~all"; \\
TTL="1"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","ttl":'"$TTL"'}' \\
    | python -m json.tool;

Alternatively, to limit the data that is displayed use:

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="TXT"; \\
NAME="@"; \\
CONTENT="v=spf1 include:\_spf.google.com ~all"; \\
TTL="1"; \\
ALL="false"; \\
curl -X POST "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","ttl":'"$TTL"'}' \\
    | if $ALL; then python -m json.tool; else python -c "import sys,json;data=json.loads(sys.stdin.read()); print('Type: ' + data\['result']\['type'] + '\\n'+'DNS ID: ' + data\['result']\['id'] if data\['success'] else 'ERROR:' + data\['errors']\[0]\['message'])"; fi

The Cloudflare dashboard now shows:

[![](https://www.tech-otaku.com/wp-content/uploads/4592-05-cloudflare-dashboard-showing-dns-records-for-dummy-com.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-05-cloudflare-dashboard-showing-dns-records-for-dummy-com.png)

Cloudflare dashboard showing the newly created **TXT** record for example.com

### 4.3. LIST, UPDATE & DELETE EXISTING DNS RECORDS

### 4.3.1 List All DNS Records for a Zone

To list all the DNS records associated with the zone record for `example.com`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
curl -X GET "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | python -m json.tool;

A far better alternative perhaps is to use the code below which limits the output to specific data and is useful if you need to quickly get the ID of an existing DNS record in order to update or delete it.

Alternative Code

Alternative Output (Success)

Alternative Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
curl -X GET "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H 'Content-Type: application/json' \\
    | python -c $'import sys,json\\ndata=json.loads(sys.stdin.read())\\nif data\["success"]:\\n\\tfor dict in data\["result"]:print("ID:" + dict\["id"] + ", Type:" + dict\["type"] + "," + "Name:" + dict\["name"] + "," + "Content:" + dict\["content"])\\nelse:print("ERROR(" + str(data\["errors"]\[0]\["code"]) + "):" + data\["errors"]\[0]\["message"])'

### 4.3.2 List DNS Records for a Zone Based on DNS Record Name

To list all the DNS records associated with the zone record for `example.com` whose name is `example.com`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
NAME="example.com"; \\
curl -X GET "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?name=$NAME](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?name=$NAME)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | python -m json.tool;

### 4.3.3 List DNS Records for a Zone Based on DNS Record Type

To list all the DNS records associated with the zone record for `example.com` whose type is `MX`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
TYPE="MX"; \\
curl -X GET "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?type=$TYPE](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?type=$TYPE)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | python -m json.tool;

### 4.3.4 List DNS Records for a Zone Based on DNS Record Name and Type

To list all the DNS records associated with the zone record for `example.com` whose name is `example.com` and type is `MX`, use the following code:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
NAME="example.com"; \\
TYPE="MX"; \\
curl -X GET "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?name=$NAME&type=$TYPE](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?name=$NAME&type=$TYPE)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | python -m json.tool;

### 4.3.5 Update an Individual DNS Record

To update an individual DNS record we need to pass its unique ID to the Cloudflare API. So, to update the CNAME record for `www.example.com` so that `PROXIED` is changed to false and `TTL` is changed to 2 minutes, use the following code. Note that even though `TYPE`, `NAME` and `CONTENT` are not changing, they still need to be included otherwise the Cloudflare API returns an error:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
DNS_ID="7bdb2e46037df332e5abdd45f8f981f5"; \\
TYPE="CNAME"; \\
NAME="www"; \\
CONTENT="example.com"; \\
PROXIED="false"; \\
TTL="120";\\
curl -X PUT "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$DNS_ID](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$DNS_ID)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    \--data '{"type":"'"$TYPE"'","name":"'"$NAME"'","content":"'"$CONTENT"'","proxied":'"$PROXIED"',"ttl":'"$TTL"'}' \\
    | python -m json.tool;

[![](https://www.tech-otaku.com/wp-content/uploads/4592-10-cloudflare-dashboard-showing-dns-record-pre-update-for-dummy-com.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-10-cloudflare-dashboard-showing-dns-record-pre-update-for-dummy-com.png)

DNS CNAME record pre-update for example.com

[![](https://www.tech-otaku.com/wp-content/uploads/4592-11-cloudflare-dashboard-showing-post-update-dns-records-for-dummy-com.png)
](https://www.tech-otaku.com/wp-content/uploads/4592-11-cloudflare-dashboard-showing-post-update-dns-records-for-dummy-com.png)

DNS CNAME record post-update for example.com

### 4.3.6 Delete an Individual DNS Record

To delete an individual DNS record associated with the zone record for `example.com`, use the following code. Note thet the DNS record’s unique ID needs to be included:

Code

Output (Success)

Output (Failure)

EMAIL="steve@example.com"; \\
KEY="08n46q4ofo0v5pc3u3g3eu517o69axu8s6ml4"; \\
ZONE_ID="8b717207bcee4047af2e9dff95832996"; \\
DNS_ID="7bdb2e46037df332e5abdd45f8f981f5"; \\
curl -X DELETE "[https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$DNS_ID](https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$DNS_ID)" \\
    \-H "X-Auth-Email: $EMAIL" \\
    \-H "X-Auth-Key: $KEY" \\
    \-H "Content-Type: application/json" \\
    | python -m json.tool;

![](https://0.gravatar.com/avatar/f360e6aeee187fd971055a98928e4727?s=70&d=mm&r=g)

A native Brit exiled in Japan, Steve spends too much of his time struggling with the Japanese language, dreaming of fish & chips and writing the occasional blog post he hopes others will find helpful. 

 [https://www.tech-otaku.com/web-development/using-cloudflare-api-manage-dns-records/#43](https://www.tech-otaku.com/web-development/using-cloudflare-api-manage-dns-records/#43) 
 [https://www.tech-otaku.com/web-development/using-cloudflare-api-manage-dns-records/#43](https://www.tech-otaku.com/web-development/using-cloudflare-api-manage-dns-records/#43)
