### Example: Obtaining all Experiments from a BioSource

The following code is an example of how to select all colon tissue samples, and then, how to list all experiments that contains peaks associated with the selected samples.

```python
user_key = "anonymous_key"
(s, related) = server.get_biosource_related("colon", user_key)
related_names = [x[1] for x in related] # get BioSource names
(s, samples) = server.list_samples(related_names, {}, user_key)
samples_id = [x[0] for x in samples] # get samples ID
print server.list_experiments("hg19", "peaks", None, None, samples_id,
                              None, None, user_key)
```

The result should look like: (partial output)

```python
['okay', [['e48272', 'ENCFF001COV'], ['e48575', 'ENCFF001COX'],
          ['e48791', 'ENCFF001WEK'], ['e48806', 'ENCFF000AEO'],
          ['e44396', 'ENCFF000PBS'], ['e44395', 'ENCFF001UER']]]
```
