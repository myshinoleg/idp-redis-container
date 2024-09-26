docker-exec = docker compose -f docker-compose.yml -p idp-redis-container exec

redis_exec = $(docker-exec) redis

#==================== Установка и получение значения ======================
set_get:
	$(redis_exec) redis-cli SET testkey 1234
	$(redis_exec) redis-cli GET testkey

#==================== Удаление и проверка наличия ======================
del:
	$(redis_exec) redis-cli del testkey
exists:
	$(redis_exec) redis-cli exists testkey

#==================== Установка и переименование значения ======================
set_rename_get:
	$(redis_exec) redis-cli SET testkey 1234
	$(redis_exec) redis-cli rename testkey newKey
	$(redis_exec) redis-cli GET newKey

#==================== Задаем время жизни ======================
ttl:
	$(redis_exec) redis-cli ttl newKey
expire:
	$(redis_exec) redis-cli expire newKey 123
ttl:
	$(redis_exec) redis-cli ttl newKey
get:
	$(redis_exec) redis-cli GET newKey

#==================== Списки ======================
rpush_lpop:
	$(redis_exec) redis-cli rpush listKey 555566
	$(redis_exec) redis-cli lpop listKey

#==================== Множества ======================
sets:
	$(redis_exec) redis-cli sadd sKey 6677
	$(redis_exec) redis-cli sadd sKey 5544
	$(redis_exec) redis-cli scard sKey
	$(redis_exec) redis-cli spop sKey

#==================== хеш-таблицы ======================
hash:
	$(redis_exec) redis-cli hset hKey hashName hashValue
	$(redis_exec) redis-cli hset hKey hashName hashValue2
	$(redis_exec) redis-cli hkeys hKey
	$(redis_exec) redis-cli hvals hKey
	$(redis_exec) redis-cli hgetall hKey
	$(redis_exec) redis-cli hdel hKey hashName
	$(redis_exec) redis-cli hgetall hKey