# General operation on redis 
1. sudo apt-get install redis -> to install redis in your system 
2. redis-server -> to start server 
3. redis-cli -> to access redis cli 
4. set <key> <value> -> to set key value pair 
5. get <key> -> to get particular key value 
6. del <key> -> to delete key with its associated value 
7. exists <key> -> to check if key exists or not 
8. keys * -> to list all the keys 
9. flushall -> to flush all the key-value from the redis
10. ttl <key> -> to check key will be remain on the redis 
11. expire <key> <time-in-sec> -> to expire key in redis after time 
12. setex <key> <time> <value> -> to set key-value with expire time . 

# List in redis 
1. lpush <key> value -> to push single value on the key
2. lpush <key> [value1, value2, value3, .....] -> to push more then one value to list
3. lrange <key> 0 -1 -> to print the entire list for the key
4. lindex <key> <index> -> to get the value from the particular index ( error on out-of-bound ) 
5. lpop <key> -> to pop the left most element from the list
6. rpop <key> -> to pop the right most element from the list
7. lset <key> <index> <value> -> to set value on the list on the specific position 

#set in redis 
1. sadd <key> <value> -> to add key-value in set
2. sadd <key> [val1, val2, val3, ....] -> to add multiple values for a key
3. smember <key> -> to get the value for the particular key
4. srem <key> <value> -> to remove the particular key-value
5. srem <key> [val1, val2, val3,.....] -> to remove list of values

#Hashes in redis 
1. hset <key> <field-name> <field-value> -> to set the person field value that is name with value dew
2. hget <key> <field-name> -> to get the particular field of the key
3. hgetall <key> -> to get the particular key values
4. hexist <key> <field-name> -> to check the particular field exists for the key

# Rules in redis 
1. set command stores key-value in string by default
2. get command is only used for getting key-value of string only type
