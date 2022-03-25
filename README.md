# Cobalt Strike Beacon Dataset

## What is it?

* Open Dataset containing 128340 rows of Cobalt Strike Beacon metadata as JSON-lines
* Historical Beacon data spanning back to 2018
* Team Server metadata (ip address, port, TLS certificate)
* Enriched with GeoIP + ASN data
* Original Beacon configuration block (deobfuscated) available in the `config_block` field
* All binary data fields are base64 encoded

## How to load the data?

To overcome the filesize limit of 100mb on GitHub, the dataset is split by year as:

 * `beacons-<year>.jsonl.gz`

We recommend using Pandas to load the dataset. Example to load a single year:
```python
>>> import pandas as pd
>>> df = pd.read_json("beacons-2022.jsonl.gz", lines=True)
```

Or to load everything as one big DataFrame:
```python
>>> import pandas as pd
>>> from pathlib import Path
>>> gen_df = (pd.read_json(fname, lines=True) for fname in Path().glob("beacons-*.jsonl.gz"))
>>> df = pd.concat(gen_df, ignore_index=True)
```

See also our accompanying [Jupyter notebook](notebook.ipynb).

## How does the data look like?

The dataset is a `gzip` compressed `.jsonl` file, i.e. each line is a JSON. Here is an example record:

```shell
$ zcat beacons-2018.jsonl.gz | head -n 1 | jq .
{
  "digest": {
    "md5": "d835cb78a451e0698e6b1a260bcc7d0f",
    "sha1": "c6bf7773eed78bc9c816856e168e8ba5275594a0",
    "sha256": "9be990a703227bee7be61c6713bda084a8f095eddf326c817ccdfa61454e76ab"
  },
  "filesize": 196608,
  "tls_subject": null,
  "tls_issuer": null,
  "tls_fingerprint": {
    "md5": null,
    "sha1": null,
    "sha256": null
  },
  "collected_from_ip": "217.182.71.175",
  "collected_from_port": 443,
  "collected_dt": "2018-07-04T00:00:00.000000",
  "domains": [
    "a0.awsstatic.com",
    "images.instagram.com",
    "media.tumblr.com",
    "cdn.zendesk.com"
  ],
  "max_setting_enum": 20,
  "beacon_version": "Cobalt Strike 3.5 (Sep 22, 2016)",
  "xorencoded": 0,
  "config_block": "AAEAAQACAAgAAgABAAIBuwADAAIABAAAAfQABAACAAQAEAAoAAUAAQACABQABgABAAIA/wAHAAMBADCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAg5WBcgWVRo+1nxVSVMT1ZJbBy57JeEIQjJIHZzw6n5BCPKGuwxjs17WPdLMIzY+osrrfFmB1N1pYZ29Lw+M9WyhyoNxPtD5MtFsP7z88oXzTZqOXgveD5dc4U9Jfbhv6tNGimS/JdmKBLF9OhoXkBOgryePBIbFz3IufvDTjX68CAwEAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACAADAQBhMC5hd3NzdGF0aWMuY29tLC9fX3V0bS5naWYsaW1hZ2VzLmluc3RhZ3JhbS5jb20sL19fdXRtLmdpZixtZWRpYS50dW1ibHIuY29tLC9fX3V0bS5naWYsY2RuLnplbmRlc2suY29tLC9fX3V0bS5naWYAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAkAAwCATW96aWxsYS81LjAgKGNvbXBhdGlibGU7IE1TSUUgOS4wOyBXaW5kb3dzIE5UIDYuMTsgV2luNjQ7IHg2NDsgVHJpZGVudC81LjApAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACgADAEAvX19fdXRtLmdpZgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAsAAwEAAAAABAAAAAIAAAAPAAAAAgAAAA8AAAACAAAACgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMAAMBAAAAAAkAAAASdXRtYWM9VUEtMjIwMjYwNC0yAAAACQAAAAd1dG1jbj0xAAAACQAAABB1dG1jcz1JU08tODg1OS0xAAAACQAAAA91dG1zcj0xMjgweDEwMjQAAAAJAAAADHV0bXNjPTMyLWJpdAAAAAkAAAALdXRtdWw9ZW4tVVMAAAAKAAAAI0hvc3Q6IGQydzZjNnhyd3NrMXpyLmNsb3VkZnJvbnQubmV0AAAABwAAAAAAAAAIAAAAAgAAAAZfX3V0bWEAAAAFAAAABXV0bWNjAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADQADAQAAAAAKAAAAJkNvbnRlbnQtVHlwZTogYXBwbGljYXRpb24vb2N0ZXQtc3RyZWFtAAAABwAAAAAAAAACAAAABlVBLTIyMAAAAAEAAAACLTIAAAAFAAAABXV0bWFjAAAACQAAAAd1dG1jbj0xAAAACQAAABB1dG1jcz1JU08tODg1OS0xAAAACQAAAA91dG1zcj0xMjgweDEwMjQAAAAJAAAADHV0bXNjPTMyLWJpdAAAAAkAAAALdXRtdWw9ZW4tVVMAAAAKAAAAI0hvc3Q6IGQydzZjNnhyd3NrMXpyLmNsb3VkZnJvbnQubmV0AAAABwAAAAEAAAAEAAAAAAAAAAAAAA4AAwBAcnVuZGxsMzIuZXhlAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAPAAMAgFxcJXNccGlwZVxtc2FnZW50XyV4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABMAAgAEAAAAAAAUAAIABAAAAAAAEAABAAIAAAARAAEAAgAAABIAAQACAAAAAA==",
  "cert_der": null,
  "xorkey": "0x69",
  "stage_prepend": null,
  "stage_append": null,
  "magic_pe": "UEU=",
  "magic_mz": "TVo=",
  "pe_compile_stamp": 1474078205,
  "pe_export_stamp": 1474078204,
  "SETTING_PROTOCOL": 8,
  "SETTING_PROTOCOL_TEXT": "BeaconProtocol.https",
  "SETTING_PORT": 443,
  "SETTING_SLEEPTIME": 500,
  "SETTING_MAXGET": 1048616,
  "SETTING_JITTER": 20,
  "SETTING_MAXDNS": 255,
  "SETTING_PUBKEY": "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCDlYFyBZVGj7WfFVJUxPVklsHLnsl4QhCMkgdnPDqfkEI8oa7DGOzXtY90swjNj6iyut8WYHU3Wlhnb0vD4z1bKHKg3E+0Pky0Ww/vPzyhfNNmo5eC94Pl1zhT0l9uG/q00aKZL8l2YoEsX06GheQE6CvJ48EhsXPci5+8NONfrwIDAQAB",
  "SETTING_PUBKEY_SHA256": "bc748561006feb7c46c34870a60ddf5c2b43ed56616c6a9f62a83114d7c76127",
  "SETTING_DOMAINS": "YTAuYXdzc3RhdGljLmNvbSwvX191dG0uZ2lmLGltYWdlcy5pbnN0YWdyYW0uY29tLC9fX3V0bS5naWYsbWVkaWEudHVtYmxyLmNvbSwvX191dG0uZ2lmLGNkbi56ZW5kZXNrLmNvbSwvX191dG0uZ2lm",
  "SETTING_USERAGENT": "TW96aWxsYS81LjAgKGNvbXBhdGlibGU7IE1TSUUgOS4wOyBXaW5kb3dzIE5UIDYuMTsgV2luNjQ7IHg2NDsgVHJpZGVudC81LjAp",
  "SETTING_SUBMITURI": "L19fX3V0bS5naWY=",
  "SETTING_C2_RECOVER": [
    "print=True",
    "prepend=15",
    "prepend=15",
    "prepend=10"
  ],
  "SETTING_C2_REQUEST": [
    "_PARAMETER=b'utmac=UA-2202604-2'",
    "_PARAMETER=b'utmcn=1'",
    "_PARAMETER=b'utmcs=ISO-8859-1'",
    "_PARAMETER=b'utmsr=1280x1024'",
    "_PARAMETER=b'utmsc=32-bit'",
    "_PARAMETER=b'utmul=en-US'",
    "_HEADER=b'Host: d2w6c6xrwsk1zr.cloudfront.net'",
    "BUILD=metadata",
    "NETBIOS=True",
    "PREPEND=b'__utma'",
    "PARAMETER=b'utmcc'"
  ],
  "SETTING_C2_POSTREQ": [
    "_HEADER=b'Content-Type: application/octet-stream'",
    "BUILD=metadata",
    "PREPEND=b'UA-220'",
    "APPEND=b'-2'",
    "PARAMETER=b'utmac'",
    "_PARAMETER=b'utmcn=1'",
    "_PARAMETER=b'utmcs=ISO-8859-1'",
    "_PARAMETER=b'utmsr=1280x1024'",
    "_PARAMETER=b'utmsc=32-bit'",
    "_PARAMETER=b'utmul=en-US'",
    "_HEADER=b'Host: d2w6c6xrwsk1zr.cloudfront.net'",
    "BUILD=output",
    "PRINT=True"
  ],
  "SETTING_SPAWNTO": "cnVuZGxsMzIuZXhl",
  "SETTING_PIPENAME": "XFwlc1xwaXBlXG1zYWdlbnRfJXg=",
  "SETTING_DNS_IDLE": 0,
  "SETTING_DNS_IDLE_IP": "0.0.0.0",
  "SETTING_DNS_SLEEP": 0,
  "SETTING_KILLDATE_YEAR": 0,
  "SETTING_KILLDATE_MONTH": 0,
  "SETTING_KILLDATE_DAY": 0,
  "country": "France",
  "country_code": "FR",
  "city": null,
  "longitude": 2.3387,
  "latitude": 48.8582,
  "asn": "16276",
  "org": "OVH SAS"
}
```

