docker-exec = docker compose -f docker-compose.yml -p idp-redis-container exec

mongo_exec = $(docker-exec) mongo mongosh -u "root" -p "root"  --authenticationDatabase "admin"

#==================== Установка и получение значения ======================
insert_find:
	$(mongo_exec) --eval 'db.users.insertOne({"name": "Oleg", "age": 29, "skill": ["php", "mongodb"]})' users
	$(mongo_exec) --eval 'db.users.find()' users

#==================== Отображение списка коллекций ======================
show:
	$(mongo_exec) --eval 'show collections'

#==================== Проекция и лимит ======================
find:
	$(mongo_exec) --eval 'db.users.find({name: "Oleg"}, {"skill": 1}).limit(3)' users

#==================== Агрегация ======================
count:
	$(mongo_exec) --eval 'db.users.find({name: "Oleg"}, {"skill": 1}).count()' users
