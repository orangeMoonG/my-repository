fastgpt

deepseek Api-key
sk-fe782edc7ccf48a392edfc4d28bac2f0

text-embeddings-v3 Api-key
sk-f90a5f8ba0f1495cb0acce8256427ad6

curl --location 'https://dashscope.aliyuncs.com/compatible-mode/v1/embeddings' \
--header "Authorization: Bearer $DASHSCOPE_API_KEY" \
--header 'Content-Type: application/json' \
--data '{
    "model": "text-embedding-v3",
    "input": "风急天高猿啸哀，渚清沙白鸟飞回，无边落木萧萧下，不尽长江滚滚来",  
    "dimension": "1024",  
    "encoding_format": "float"
}'