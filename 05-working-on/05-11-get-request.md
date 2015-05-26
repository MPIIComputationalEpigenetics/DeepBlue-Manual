## Retieving Data

Using [get_request_status](http://deepblue.mpi-inf.mpg.de/api.php#api-get_request_status) the status of a request can be seen. As soon as the request is processed, the requested data can be retrieved using [get_request_data](http://deepblue.mpi-inf.mpg.de/api.php#api-get_request_data).

```python
>>> print server.get_regions(query_id, "CHROMOSOME,START,END,VALUE,STRAND", user_key)
['okay', 'r1']
>>> print server.get_request_regions('r1', user_key)
Request ID r1 was not finished. Please, check its status.
>>> print server.get_request_status('r1', user_key)
['okay', ['running', '']]
>>> print server.get_request_status('r1', user_key)
['okay', ['done', '']]
>> print server.get_request_regions('r1', user_key)
```
