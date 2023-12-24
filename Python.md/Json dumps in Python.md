export data from mongo , you will face `Type error` when you use `json.dumps(doc, ensure_ascii=False)` sometimes. 

```bash 
TypeError: Object of type ObjectId is not JSON serializable
```

this because your doc contains  `mongo_id`  that is not `JSON serializable`
you must  serialize this k-v item by yourself.
1. you can ues `lambda` 
```python
   oid_to_str = lambda doc: {
    k: str(v) if isinstance(v, ObjectId) else v for k, v in doc.items()
}

```
2. wirete a  class 
```python
import json
from bson import ObjectId

class JSONEncoder(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, ObjectId):
            return str(o)
        return json.JSONEncoder.default(self, o)

...
# specific this class to tell json.dumps how to serialize json!! 
f.write(f"{json.dumps(doc, ensure_ascii=False, cls=JSONEncoder)}\n")
```

congradulation!  it work! 
