docker-exec = docker compose -f docker-compose.yml -p idp-redis-container exec

header = -H 'Content-Type: application/json'

xput = $(docker-exec) elasticsearch curl -XPUT
xget = $(docker-exec) elasticsearch curl -XGET
xpost = $(docker-exec) elasticsearch curl -XPOST

bulk = $(xpost) $(header) "http://localhost:9200/dev/_bulk?pretty"
search = $(xpost) $(header) "http://localhost:9200/dev/_doc/_search?pretty" -d

#==================== Создание  индекса ======================
put_index:
	$(xput) "localhost:9200/dev"

#==================== Маппинг полей ======================
mapping:
	$(xput) $(header) "http://localhost:9200/dev/_mappings" -d '{"properties": {"birthdate": {"type": "date","format": "dateOptionalTime"},"location": {"type": "geo_point"},"name": {"type": "text"},"skill": {"type": "text"}}}'

#==================== Накат дампа ======================
insert:
	$(bulk) -d'\
	{"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Vova", "birthdate":"1999-10-1", "skill":"php", "location":"46.22,-68.45"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Oleg", "birthdate":"1999-11-2", "skill":"php", "location":"45.21,-68.35"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Goga", "birthdate":"1998-10-3", "skill":"php", "location":"45.16,-63.58" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Zhora", "birthdate":"2002-5-20", "skill":"php",  "location":"45.22,-68.53"}\
    {"index":{"_index":"dev","_type":"athlete"}}\
    {"name":"Garic", "birthdate":"2002-2-28", "skill":"php", "location":"46.22,-68.85"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Afonya", "birthdate":"2000-9-9", "skill":"php", "location":"45.12,-68.35"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Zhenya", "birthdate":"2000-4-1", "skill":"php", "location":"46.12,-68.55"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Winston", "birthdate":"1998-3-1", "skill":"php", "location":"46.25,-68.55" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Alex", "birthdate":"1999-10-1", "skill":"php", "location":"46.22,-68.45"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Biba", "birthdate":"1999-11-2", "skill":"php", "location":"45.21,-68.35"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Boba", "birthdate":"1998-10-3", "skill":"php", "location":"45.16,-63.58" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Huisunwin", "birthdate":"2002-5-20", "skill":"php",  "location":"45.22,-68.53"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Ashot", "birthdate":"2002-2-28", "skill":"php", "location":"46.22,-68.85"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Ibragim", "birthdate":"2000-9-9", "skill":"php", "location":"45.12,-68.35"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Jacki", "birthdate":"2000-4-1", "skill":"php", "location":"46.12,-68.55"}\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Vaas", "birthdate":"1998-3-1", "skill":"php", "location":"46.25,-68.55" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Mechenyi", "birthdate":"1998-3-1", "skill":"Go", "location":"46.25,-68.55" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Sidorovich", "birthdate":"1998-3-1", "skill":"Go", "location":"46.25,-68.55" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Strelok", "birthdate":"1998-3-1", "skill":"Java", "location":"46.25,-68.55" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Wombat", "birthdate":"1998-3-1", "skill":"C", "location":"46.25,-68.55" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Winki", "birthdate":"1998-3-1", "skill":"Python", "location":"46.25,-68.55" }\
    {"index":{"_index":"dev","_type":"_doc"}}\
    {"name":"Lewis", "birthdate":"1998-3-1", "skill":"Python", "location":"46.25,-68.55" }\
    '

#==================== Агрегация по полю skill. Работает, но по-умолчанию отключена ======================
aggregate_by_term:
	$(xget) $(header) "http://localhost:9200/dev/_doc/_search?pretty" -d '{"size": 0, "aggregations":{"aggr_name":{"terms":{"field": "skill"}}}}'

#==================== получить количество документов подходящих под фильтр ======================
aggregate_by_filters:
	$(xget) $(header) "http://localhost:9200/dev/_doc/_search?pretty" -d '{"size": 0,"aggregations":{"aggr_name":{"filters" : {"filters" : [{ "match" : {"skill": "php"}}]}}}}'

#==================== агрегация и поиск по фильтру ======================
search_by_term:
	$(xget) $(header) "http://localhost:9200/dev/_doc/_search?pretty" -d '{"aggregations": {"alias": {"filter": {"term": { "skill": "php" }}}}}'

#==================== Группировка по скрипту и вывод результата в рамках range. todo Пофиксить получение текущей даты.  ======================
aggregate_script:
	$(search) '{"size": 0,"aggregations": {"age_ranges": {"range": {"script": "DateTime nowZdt = DateTime.value.year; nowZdt - doc[\"birthdate\"].date.year","ranges": [{"from": 22,"to": 25}]}}}}'



