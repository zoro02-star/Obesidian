## injections ->
```
# Single URL
nuclei -t ssti-detection.yaml -u "https://target.com/page?user=John&lang=en" -fuzz

# From a list of URLs
nuclei -t ssti-detection.yaml -l urls.txt -fuzz

# single URL
nuclei -t xss-detection.yaml -u "https://target.com/search?q=test" -fuzz

# URL list
nuclei -t xss-detection.yaml -l urls.txt -fuzz -H "Custom-Header: value"

# increase concurrency for large lists
nuclei -t xss-detection.yaml -l urls.txt -fuzz -c 50 -rate-limit 100
```

```
# URL list (recommended — add -timeout for time-based rules)
nuclei -t cmdi-detection.yaml -l urls.txt -fuzz -timeout 15

# Single URL
nuclei -t cmdi-detection.yaml -u "https://target.com/ping?host=1.1.1.1" -fuzz -timeout 15

# Quiet mode, only confirmed findings
nuclei -t cmdi-detection.yaml -l urls.txt -fuzz -timeout 15 -silent
```

```
# URL list
nuclei -t ssi-esi-injection.yaml -l urls.txt -fuzz

# Single target (Apache .shtml page)
nuclei -t ssi-esi-injection.yaml -u "https://target.com/page.shtml?name=test" -fuzz

# Verbose
nuclei -t ssi-esi-injection.yaml -l urls.txt -fuzz -v
```

```
# Basic run
nuclei -t xxe-detection.yaml -u https://target.com/api/endpoint

# With your own interactsh server
nuclei -t xxe-detection.yaml -u https://target.com -iserver https://your.interactsh.com

# Run only blind OOB tests
nuclei -t xxe-detection.yaml -u https://target.com -id xxe-blind-oob,xxe-blind-oob-param-entity
```


## databases ->

```
# Full scan against a target
nuclei -t bigquery-sqli-detect.yaml -u https://target.com/api/data

# Only error-based tests (no OOB needed)
nuclei -t bigquery-sqli-detect.yaml -u https://target.com \
  -id bigquery-sqli-error-single-quote,bigquery-sqli-error-project-id,bigquery-sqli-error-div-by-zero

# OOB blind tests (requires interactsh token)
nuclei -t bigquery-sqli-detect.yaml -u https://target.com \
  -id bigquery-sqli-oob-dns,bigquery-sqli-boolean-substring \
  -iserver https://your.interactsh.com -itoken YOUR_TOKEN

# Fuzz multiple params at once
nuclei -t bigquery-sqli-detect.yaml -l urls.txt -v
```

```
# XXE
nuclei -t xxe-injection-detect.yaml -l urls.txt -v

# BigQuery SQLi
nuclei -t bigquery-sqli-detect.yaml -l urls.txt -v

# Cassandra CQL
nuclei -t cassandra-cql-injection.yaml -l urls.txt -v

# All three at once
nuclei -t . -l urls.txt -tags sqli,xxe,cassandra,bigquery -v

# OOB tests with interactsh
nuclei -t . -l urls.txt -iserver https://your.interactsh.com -itoken YOUR_TOKEN
```

```
# Full scan with your URL list
nuclei -t db2-sql-injection.yaml -l urls.txt -v

# Error and fingerprint tests only (safe/passive)
nuclei -t db2-sql-injection.yaml -l urls.txt \
  -id db2-sqli-single-quote-fingerprint,db2-sqli-comment-probe,db2-sqli-error-xmlagg

# Time-based only (slow — run separately)
nuclei -t db2-sql-injection.yaml -l urls.txt \
  -id db2-sqli-time-based-heavy-query -timeout 30

# IBM i / AS400 RCE test (requires interactsh)
nuclei -t db2-sql-injection.yaml -l urls.txt \
  -id db2-sqli-qcmdexc-rce \
  -iserver https://your.interactsh.com -itoken YOUR_TOKEN

# Post body injection tests
nuclei -t db2-sql-injection.yaml -l urls.txt \
  -id db2-sqli-post-api
```

```
# Full scan
nuclei -t graphql-injection-detect.yaml -l urls.txt -v

# Introspection checks only
nuclei -t graphql-injection-detect.yaml -l urls.txt \
  -id graphql-introspection-enabled,graphql-introspection-full-schema,graphql-sensitive-type-definition

# Injection tests only
nuclei -t graphql-injection-detect.yaml -l urls.txt \
  -id graphql-sqli-single-quote,graphql-sqli-url-parameter,graphql-nosql-injection-regex,graphql-nosql-projection-leak
```

```
# Full scan
nuclei -t mssql-sql-injection.yaml -l urls.txt -v

# Error-based only (fastest, no timing)
nuclei -t mssql-sql-injection.yaml -l urls.txt \
  -id mssql-sqli-single-quote-fingerprint,mssql-sqli-error-convert-version,mssql-sqli-error-dbname-leak

# Time-based only (increase timeout)
nuclei -t mssql-sql-injection.yaml -l urls.txt \
  -id mssql-sqli-time-based-waitfor -timeout 15

# OOB tests (needs interactsh)
nuclei -t mssql-sql-injection.yaml -l urls.txt \
  -id mssql-sqli-oob-dns \
  -iserver https://your.interactsh.com -itoken YOUR_TOKEN
```

```
# Full scan
nuclei -t nosql-injection-detect.yaml -l urls.txt -v

# Auth bypass tests only
nuclei -t nosql-injection-detect.yaml -l urls.txt \
  -id nosqli-auth-bypass-ne-post-json,nosqli-auth-bypass-ne-post-form,nosqli-auth-bypass-in-operator

# Fingerprint + error only (safest)
nuclei -t nosql-injection-detect.yaml -l urls.txt \
  -id nosqli-error-fingerprint,nosqli-operator-injection-get-probe

# Blind differential tests only
nuclei -t nosql-injection-detect.yaml -l urls.txt \
  -id nosqli-blind-boolean-get-differential,nosqli-blind-boolean-post-differential,nosqli-regex-data-extraction-probe
```

```
# Full scan
nuclei -t oracle-sql-injection.yaml -l urls.txt -v

# Error-based only (fastest)
nuclei -t oracle-sql-injection.yaml -l urls.txt \
  -id oracle-sqli-single-quote-fingerprint,oracle-sqli-error-version-utlinaddr,oracle-sqli-error-ctxsys,oracle-sqli-error-xmltype

# Time-based only (set longer timeout)
nuclei -t oracle-sql-injection.yaml -l urls.txt \
  -id oracle-sqli-time-based-dbms-pipe -timeout 15

# OOB tests (needs interactsh)
nuclei -t oracle-sql-injection.yaml -l urls.txt \
  -id oracle-sqli-oob-dns-xmltype,oracle-sqli-oob-utl-http \
  -iserver https://your.interactsh.com -itoken YOUR_TOKEN
```

```
# Full scan
nuclei -t postgresql-sql-injection.yaml -l urls.txt -v

# Error-based only (fastest)
nuclei -t postgresql-sql-injection.yaml -l urls.txt \
  -id pgsql-sqli-single-quote-fingerprint,pgsql-sqli-error-cast-version,pgsql-sqli-error-cast-db-user

# Time-based only (set longer timeout)
nuclei -t postgresql-sql-injection.yaml -l urls.txt \
  -id pgsql-sqli-time-based-pg-sleep -timeout 15

# OOB tests (needs interactsh)
nuclei -t postgresql-sql-injection.yaml -l urls.txt \
  -id pgsql-sqli-oob-copy-program-dns \
  -iserver https://your.interactsh.com -itoken YOUR_TOKEN

# All injection templates at once
nuclei -t . -l urls.txt \
  -tags sqli,postgresql,mssql,oracle,nosql,db2,cassandra,bigquery,graphql,xxe -v
```

```
# Full scan
nuclei -t sqlite-sql-injection.yaml -l urls.txt -v

# Error + fingerprint only (fastest, safest)
nuclei -t sqlite-sql-injection.yaml -l urls.txt \
  -id sqlite-sqli-single-quote-fingerprint,sqlite-sqli-union-version,sqlite-sqli-union-schema-dump

# Blind differential tests only
nuclei -t sqlite-sql-injection.yaml -l urls.txt \
  -id sqlite-sqli-blind-boolean-table-count,sqlite-sqli-blind-hex-substring,sqlite-sqli-blind-substr-substring

# Time-based only (set longer timeout — RANDOMBLOB can be slow)
nuclei -t sqlite-sql-injection.yaml -l urls.txt \
  -id sqlite-sqli-time-based-randomblob -timeout 30

# Run all database injection templates together
nuclei -t . -l urls.txt \
  -tags sqli,sqlite,postgresql,mssql,oracle,nosql,db2,cassandra,bigquery,graphql,xxe -v
```