# ansible-cassandra
Ansible provisioning/maintenance tasks for Cassandra. Can be used to install & manage Configuratio for a DSE/Apache based Cassandra cluster, Solr and Spark

Usage:

1. Create the servers for Apache Cassandra or DSE cassandra/Spark/Solr 
2. Define an Ansible inventory (see inventory/example.hosts) for your environment
   eg: Check the Templates
   - apachecassandraTemplate.hosts for Apache Cassandra Install Edition
   - DSEtemplate.hosts for DSE Cassandra Install Edition, this includes Solr and Spark hosts, in case if you don't want to enable Spark and Solr leave it balnk 
3. Run the playbook to install Cassandra
  eg:  ansible-playbook -i apachecassandraTemplate.hosts cassandraDeploy.yml -> Apache Cassandra Installation
       ansible-playbook -i DSEtemplate.hosts cassandraDeploy.yml
4. Run the Playbool to modify Config files, every Task has given TAG name
  eg:  ansible-playbook -i DSEtemplate.hosts cassandraDeploy.yml --tags "cassandra-stop" -> to stop the cassandra services at cluster level
       ansible-playbook -i apachecassandraTemplate.hosts cassandraDeploy.yml --tags "cassandra.yaml, cassandra-env.sh, cassandra-restart" -> this will modify yaml,env.sh file and restarts cassandra 
Inventory configuration:

Inventory group | Variable | Options | Default | Description
--- | --- | --- | --- | ---
cassandra_nodes | dc | DC1, DC2, ... | - | data center of node
cassandra_nodes | rack | RAC1, RAC2, ... | - | rack of node
cassandra_nodes | repair_weekday | MON,TUE,WED,THU,FRI,SAT,SUN | - | day(s) to run repair on node
cassandra_nodes | repair_start_hour | 00-23 | 03 | hour to start cron based repair
cassandra_nodes | repair_start_minute | 00-59 | 0 | minute to start cron based repair
cassandra_nodes | seed | true, false | - | is the node a seed
cassandra_nodes | node_ip | true, false | - | IP for internal cluster communications
cassandra_nodes | SPARK_ENABLED | 1 | 0 | enable Spark on node (DSE only)
cassandra_nodes | SOLR_ENABLED | 1 | 0 | enable Solr on node (DSE only)

Requirements:
- Ansible 2.0 or later


Running:
- Check out main cassandraDeploy.yml comments for typical running options (e.g. Install, re-config..etc)

Spark setup:
Typical way of setting up the environment would be to define 2 Cassandra data centers: one for real-time transactions (plain Cassandra) and
another for analytics workloads (Cassandra with co-located Spark nodes). You can also use the playbook without installing Spark.
