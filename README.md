[![PyPI version](https://badge.fury.io/py/ipdata.svg)](https://badge.fury.io/py/ipdata) ![GitHub Workflow Status](https://img.shields.io/github/workflow/status/ipdata/python/Test%20and%20Publish%20ipdata%20to%20PyPI)

# Official Python client library and CLI for the ipdata API

This is a Python client and command line interface (CLI) for the [ipdata.co](https://ipdata.co) IP Geolocation API. ipdata offers a fast, highly-available API to enrich IP Addresses with Location, Company, Threat Intelligence and numerous other data attributes.

Note that you need an API Key to use this package. You can get a free one with a 1,500 daily request limit by [Signing up here](https://ipdata.co/sign-up.html).

Visit our [Documentation](https://docs.ipdata.co/) for more examples and tutorials.

[![asciicast](https://asciinema.org/a/371292.svg)](https://asciinema.org/a/371292)

## Installation

Install the latest version of the cli with `pip`.

```
pip install ipdata
```

or `easy_install`

```
easy_install ipdata
```

## Library Usage

You need a valid API key from ipdata to use the library. You can get a free key by [Signing up here](https://ipdata.co/sign-up.html).

Replace `test` with your API Key in the following examples.

### Looking up the calling IP Address

You can look up the calling IP address, that is, the IP address of the computer you are running this on by not passing an IP address to the `lookup` method.

```
from ipdata import ipdata
from pprint import pprint
# Create an instance of an ipdata object. Replace `test` with your API Key
ipdata = ipdata.IPData('test')
response = ipdata.lookup()
pprint(response)
```

### Looking up any IP Address

You can look up any valid IPv4 or IPv6 address by passing it to the `lookup` method.

```
from ipdata import ipdata
from pprint import pprint
# Create an instance of an ipdata object. Replace `test` with your API Key
ipdata = ipdata.IPData('test')
response = ipdata.lookup('69.78.70.144')
pprint(response)
```

<details><summary>Sample Response</summary>

```
{'asn': 'AS6167',
 'calling_code': '1',
 'carrier': {'mcc': '310', 'mnc': '004', 'name': 'Verizon'},
 'city': 'Farmersville',
 'continent_code': 'NA',
 'continent_name': 'North America',
 'count': '1506',
 'country_code': 'US',
 'country_name': 'United States',
 'currency': {'code': 'USD',
              'name': 'US Dollar',
              'native': '$',
              'plural': 'US dollars',
              'symbol': '$'},
 'emoji_flag': '🇺🇸',
 'emoji_unicode': 'U+1F1FA U+1F1F8',
 'flag': 'https://ipdata.co/flags/us.png',
 'ip': '69.78.70.144',
 'is_eu': False,
 'languages': [{'name': 'English', 'native': 'English'}],
 'latitude': 33.1659,
 'longitude': -96.3686,
 'organisation': 'Cellco Partnership DBA Verizon Wireless',
 'postal': '75442',
 'region': 'Texas',
 'region_code': 'TX',
 'status': 200,
 'threat': {'is_anonymous': False,
            'is_bogon': False,
            'is_known_abuser': False,
            'is_known_attacker': False,
            'is_proxy': False,
            'is_threat': False,
            'is_tor': False},
 'time_zone': {'abbr': 'CDT',
               'current_time': '2019-04-28T17:56:59.246755-05:00',
               'is_dst': True,
               'name': 'America/Chicago',
               'offset': '-0500'}}
```
</details>


### Getting only one field

If you only need a single data attribute about an IP address you can extract it by passing a `select_field` parameter to the `lookup` method.

```
from ipdata import ipdata
from pprint import pprint
# Create an instance of an ipdata object. Replace `test` with your API Key
ipdata = ipdata.IPData('test')
response = ipdata.lookup('8.8.8.8', select_field='organisation')
pprint(response)
```

Response

```
{'organisation': 'Google LLC', 'status': 200}
```

### Getting a number of specific fields

If instead you need to get multiple specific fields you can pass a list of valid field names in a `fields` parameter.

```
from ipdata import ipdata
from pprint import pprint
# Create an instance of an ipdata object. Replace `test` with your API Key
ipdata = ipdata.IPData('test')
response = ipdata.lookup('8.8.8.8',fields=['ip','organisation','country_name'])
pprint(response)
```

Response

```
{'country_name': 'United States',
 'ip': '8.8.8.8',
 'organisation': 'Google LLC',
 'status': 200}
```

### Bulk Lookups

The API provides a `/bulk` endpoint that allows you to look up upto 100 IP addresses at a time. This is convenient for quickly clearing your backlog.

NOTE: Alternatively it is much simpler to process bulk lookups using the `ipdata` cli's `batch` command. All you need is a csv file with a list of IP addresses and you can get your results as either a JSON file or a CSV file! See the [CLI Bulk Lookup Documentation](https://docs.ipdata.co/command-line-interface/bulk-lookups-recommended) for details.

```
from ipdata import ipdata
from pprint import pprint
# Create an instance of an ipdata object. Replace `test` with your API Key
ipdata = ipdata.IPData('test')
response = ipdata.bulk_lookup(['8.8.8.8','1.1.1.1'])
pprint(response)
```

<details><summary>Sample Response</summary>

```
{'responses': [{'asn': 'AS15169',
               'calling_code': '1',
               'city': None,
               'continent_code': 'NA',
               'continent_name': 'North America',
               'count': '1506',
               'country_code': 'US',
               'country_name': 'United States',
               'currency': {'code': 'USD',
                            'name': 'US Dollar',
                            'native': '$',
                            'plural': 'US dollars',
                            'symbol': '$'},
               'emoji_flag': '🇺🇸',
               'emoji_unicode': 'U+1F1FA U+1F1F8',
               'flag': 'https://ipdata.co/flags/us.png',
               'ip': '8.8.8.8',
               'is_eu': False,
               'languages': [{'name': 'English', 'native': 'English'}],
               'latitude': 37.751,
               'longitude': -97.822,
               'organisation': 'Google LLC',
               'postal': None,
               'region': None,
               'region_code': None,
               'threat': {'is_anonymous': False,
                          'is_bogon': False,
                          'is_known_abuser': False,
                          'is_known_attacker': False,
                          'is_proxy': False,
                          'is_threat': False,
                          'is_tor': False},
               'time_zone': {'abbr': 'CDT',
                             'current_time': '2019-04-28T18:02:48.035425-05:00',
                             'is_dst': True,
                             'name': 'America/Chicago',
                             'offset': '-0500'}},
              {'asn': 'AS13335',
               'calling_code': '61',
               'city': None,
               'continent_code': 'OC',
               'continent_name': 'Oceania',
               'count': '1506',
               'country_code': 'AU',
               'country_name': 'Australia',
               'currency': {'code': 'AUD',
                            'name': 'Australian Dollar',
                            'native': '$',
                            'plural': 'Australian dollars',
                            'symbol': 'AU$'},
               'emoji_flag': '🇦🇺',
               'emoji_unicode': 'U+1F1E6 U+1F1FA',
               'flag': 'https://ipdata.co/flags/au.png',
               'ip': '1.1.1.1',
               'is_eu': False,
               'languages': [{'name': 'English', 'native': 'English'}],
               'latitude': -33.494,
               'longitude': 143.2104,
               'organisation': 'Cloudflare, Inc.',
               'postal': None,
               'region': None,
               'region_code': None,
               'threat': {'is_anonymous': False,
                          'is_bogon': False,
                          'is_known_abuser': False,
                          'is_known_attacker': False,
                          'is_proxy': False,
                          'is_threat': False,
                          'is_tor': False},
               'time_zone': {'abbr': 'AEST',
                             'current_time': '2019-04-29T09:02:48.036287+10:00',
                             'is_dst': False,
                             'name': 'Australia/Sydney',
                             'offset': '+1000'}}],
 'status': 200}
```
</details>

## Using the ipdata CLI

### Available commands

```
ipdata --help
Usage: ipdata [OPTIONS] COMMAND [ARGS]...

  CLI for ipdata API

Options:
  --api-key TEXT  ipdata API Key
  --help          Show this message and exit.

Commands:
  batch
  info
  init
  me
```

### Initialize the cli with your API Key

You need a valid API key from ipdata to use the cli. You can get a free key by [Signing up here](https://ipdata.co/sign-up.html).

```
ipdata init <API Key>
```

You can also pass the `--api-key <API Key>` parameter to any command to specify a different API Key.

### Look up your own IP address

Running the `ipdata` command without any parameters will look up the IP address of the computer you are running the command on. Alternatively you can explicitly look up your own IP address by running `ipdata me` . You can filter the JSON response with `jq` to get any specific fields you might be interested in.


```
ipdata
```

or

```
ipdata me
```

Using `jq` to filter the responses 

```
ipdata me | jq .country_name
```

### Look up an arbitrary IP address

You can pass any valid IPv4 or IPv6 address to the `ipdata` command to look it up. In case an invalid value is passed you will get the error `Error: No such command "1..@1....1..1"`.

```
ipdata 8.8.8.8
```

### Filter results by specifying comma separated list of fields 

In case you don't want to use `jq` to filter responses to get specific fields you can instead pass a fields argument to the `ipdata` command along with a comma separated list of valid fields. Invalid fields are ignored. It is important to not include any whitespace in the list.

To access fields within nested objects eg. in the case of the `asn`, `languages`, `currency`, `time_zone` and `threat` objects, you can get a nested field by using dot notation with the name of the object and the name of the field. For example to get the time_zone name you would use `time_zone.name`, to get the time_zone abbreviation you would use `time_zone.abbr`

```
ipdata 8.8.8.8 --fields ip,country_code
```

### Batch lookup

Perhaps the most useful command provided by the CLI is the ability to process a csv file with a list of IP addresses and write the results to file as either CSV or JSON! It could be a list of tens of thousands to millions of IP addresses and it will all be processed and the results filtered to your liking!
When you use the JSON output format, the results are written to the output file you provide with one result per line. Each line being a valid and full JSON response object.
If you only need a few fields eg. only the country name you can specify a field argument with the names of the fields you want, if you combine this with the CSV output format you will get very clean results with only the data you need!

### To get full JSON responses for further processing

```
ipdata batch my_ip_backlog.csv --output geolocation_results.json
```

### Batch lookup with output to CSV file

```
ipdata batch my_ip_backlog.csv --output results.csv --output-format CSV --fields ip,country_code
```

The `--fields` option is required in case of CSV output.

#### Example Results

```
# ip,country_code
107.175.75.83,US
35.155.95.229,US
13.0.0.164,US
209.248.120.14,US
142.0.202.238,US
...
```

## Available Fields

A list of all the fields returned by the API is maintained at [Response Fields](https://docs.ipdata.co/api-reference/response-fields)

## Errors

A list of possible errors is available at [Status Codes](https://docs.ipdata.co/api-reference/status-codes)

## Tests

To run all tests

```
python -m unittest
```