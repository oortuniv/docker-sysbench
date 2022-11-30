# OORTUNIV SYSBENCH

## TL;DR
```
docker run --name sysbench oortuniv/sysbench:latest sysbench --db-driver=mysql --mysql-host=YOUR_HOST --mysql-port=YOUR_PORT --mysql-user=YOUR_USER --mysql-password=YOUR_PW /opt/oortuniv/sysbench/lua/oltp_read_write.lua prepare

docker run --name sysbench oortuniv/sysbench:latest sysbench --db-driver=mysql --mysql-host=YOUR_HOST --mysql-port=YOUR_PORT --mysql-user=YOUR_USER --mysql-password=YOUR_PW /opt/oortuniv/sysbench/lua/oltp_read_write.lua run
```

---

## Run By Script
You can specify a script to run. Through this, it is possible to make a more scalable benchmark to multiple targets. Beware of permission.
### RUN COMMAND
```
docker run --name sysbench -v YOUR_DIR:/opt/oortuniv/sysbench/src/run oortuniv/sysbench:latest
```
### YOUR_DIR/sample.sh
```
# Setting
export PGPASSWORD=postgres
SB_CMD="sysbench --db-driver=pgsql --pgsql-host=YOUR_HOST --pgsql-port=5432 --pgsql-user=postgres --pgsql-password=postgres"
PSQL_CMD="psql -h YOUR_HOST -d postgres -U postgres -c "

# Cleanup before running
$PSQL_CMD "CREATE DATABASE SBTEST;"
$SB_CMD /opt/oortuniv/sysbench/lua/oltp_read_write.lua cleanup

# Grant privileges for monitoring 
$PSQL_CMD "GRANT ALL PRIVILEGES ON DATABASE SBTEST TO postgres;"

# Run
$SB_CMD /opt/oortuniv/sysbench/lua/oltp_read_write.lua prepare
$SB_CMD --time=2592000 --threads=10 /opt/oortuniv/sysbench/lua/oltp_read_write.lua run
$SB_CMD /opt/oortuniv/sysbench/lua/oltp_read_write.lua cleanup
```

---

## LUA

All lua files are located in `/opt/oortuniv/sysbench/lua` directory in the container.

---

## LOG
All log files are located in `oortuniv/sysbench/log` directory in the container.