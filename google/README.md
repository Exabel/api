This folder contains proto buffers from Google that are not included by default
in some languages (i.e. Python).

They can be updated using the following commands
```
curl https://raw.githubusercontent.com/googleapis/googleapis/master/google/api/annotations.proto > api/annotations.proto     
curl https://raw.githubusercontent.com/googleapis/googleapis/master/google/api/http.proto > api/http.proto
curl https://raw.githubusercontent.com/googleapis/googleapis/master/google/api/field_behavior.proto > api/field_behavior.proto
curl https://raw.githubusercontent.com/googleapis/googleapis/master/google/rpc/code.proto > rpc/code.proto
curl https://raw.githubusercontent.com/googleapis/googleapis/master/google/rpc/error_details.proto > rpc/error_details.proto
curl https://raw.githubusercontent.com/googleapis/googleapis/master/google/rpc/status.proto > rpc/status.proto
```
