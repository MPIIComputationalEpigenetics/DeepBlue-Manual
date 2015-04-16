## Aggregation

DeepBlue can [aggregate](http://deepblue.mpi-inf.mpg.de/api.php#api-aggregate) groups of regions into larger regions.
The [aggregate](http://deepblue.mpi-inf.mpg.de/api.php#api-aggregate) command requires three parameters: `query_data_id`, `query_regions_id`, and the data column name that will be the aggregation pivot.

It is possible to retrieve the aggregation result with the aggregation metafield:

| Aggregation Command | Description                |
|---------------------|----------------------------|
| @AGG.MIN            | Regions minimum value      |
| @AGG.MAX            | Regions maximum value      |
| @AGG.MEDIAN         | Regions median value       |
| @AGG.MEAN           | Regions mean               |
| @AGG.VAR            | Regions variance           |
| @AGG.SD             | Regions standard deviation |
| @AGG.COUNT          | Regions count              |

Please, remember that DeepBlue does not aggregate inter-region regions, only the regions that are fully overlapped by some `query_region_id`.

In the following example, we aggregate the retrieved data into tiling regions of length 100000.
In the, end we remove the aggregated regions that do not contain any region:

```python
(status, blood_related) = server.get_biosource_related("blood", user_key)
blood_related_names = [x[1] for x in blood_related]

(status, blood_samples) = server.list_samples(blood_related_names, {"karyotype":"cancer"}, user_key)

blood_samples_ids = [x[0] for x in blood_samples]

(status, data_id) = server.select_regions(None, "hg19", "DNA Methylation", blood_samples_ids, None, None, "chr1", None, None, user_key)

(s, regions_id) = server.tiling_regions(100000, "hg19", "chr1", user_key)

(status, aggr_id) = server.aggregate(data_id, regions_id, "SCORE", user_key)

(status, aggr_filter) = server.filter_regions(aggr_id, "@AGG.COUNT", ">", "0", "integer", user_key)

(status, request_id) =  server.get_regions(aggr_filter, "CHROMOSOME,START,END,@AGG.MIN,@AGG.MAX,@AGG.MEDIAN,@AGG.MEAN,@AGG.SD,@AGG.COUNT", user_key)

(status, regions) = server.get_request_data(request_id, user_key)
```
